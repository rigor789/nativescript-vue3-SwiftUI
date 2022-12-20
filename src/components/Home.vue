<script lang="ts" setup>
import {
  ref,
  computed,
  onMounted,
  onUnmounted,
  $navigateTo,
  reactive,
} from "nativescript-vue";
import { registerSwiftUI, UIDataDriver } from "@nativescript/swift-ui";

import Details from "./Details.vue";
import { Color } from "@nativescript/core";

const counter = ref(0);
const message = computed(() => {
  return `Blank {N}-Vue app: ${counter.value}`;
});

function logMessage() {
  console.log("You have tapped the message!");
}

let interval: any;
onMounted(() => {
  console.log("mounted");
  interval = setInterval(() => counter.value++, 100);
});

onUnmounted(() => {
  console.log("unmounted");
  clearInterval(interval);
});

// @ts-ignore
declare const CardProvider: any;

interface ISwiftUIData {
  title?: string;
  titleColor?: UIColor;
  desc?: string;
  descColor?: UIColor;
  subTitle?: string;
  subTitleColor?: UIColor;
  cardBackgroundColor?: UIColor;
}

registerSwiftUI(
  "card",
  (view) => new UIDataDriver(CardProvider.alloc().init(), view)
);

const swiftUIData: ISwiftUIData = {
  title: "Hello World!",
  titleColor: new Color("white").ios,
  desc: "with NativeScript from Vue!",
  subTitle: "Heck Yeah!",
  subTitleColor: new Color('white').ios,
  cardBackgroundColor: new Color("#65adf1").ios,
};

const onSwiftUIEvent = (event: any) => {
  console.log("SWIFTUI EVENT!");
};
</script>

<template>
  <Frame>
    <Page>
      <ActionBar>
        <Label text="Home" class="font-bold text-lg" />
      </ActionBar>

      <GridLayout rows="auto, auto, auto, *" class="px-4">
        <Label
          row="1"
          class="text-xl align-middle text-center text-gray-500"
          :text="message"
          @tap="logMessage"
        />

        <Button
          row="2"
          @tap="$navigateTo(Details)"
          class="mt-4 px-4 py-2 bg-white border-2 border-blue-400 rounded-lg"
          horizontalAlignment="center"
        >
          View Details
        </Button>

        <SwiftUI
          row="3"
          swiftId="card"
          :data="swiftUIData"
          @swiftUIEvent="onSwiftUIEvent"
        />
      </GridLayout>
    </Page>
  </Frame>
</template>

<style>
/* .info {
    font-size: 20;
  } */
</style>
