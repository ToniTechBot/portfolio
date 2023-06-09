<template>
  <q-page class="noPointerEvents" @touchmove="preventScroll">
    <div class="row items-center justify-center ">
      <!-- full name + job position -->
      <div class="col-6 col-xs-12 col-sm-11 col-md-10 col-lg-6 col-xl-6 q-px-lg" style="margin-top: -4px">
        <div class="text-center">
          <div class="name text-left">
            <div class="animated-text">
              <div class="typing-text">TONI</div>
              <div class="typing-text q-pb-sm">DREGER</div>
            </div>
            <q-separator color="white" style="opacity: 0.7; margin-right: 5rem" />
            <q-separator color="white" class="q-mt-md" style="opacity: 0.7; margin-left: 5rem" />
            <div class="text-center q-pt-md  subtitle">
              {{ $t("jobPosition") }}
            </div>
          </div>
        </div>
        <!-- Buttons -->
        <div class="col-5 col-xs-12 col-sm-11 col-md-10 col-lg-6 col-xl-6 q-px-xs text-center noPointerEvents"
          style="margin-top: 0.8rem">
          <div>
            <WindowsButton color="white" :label="$t('work')" icon="mdi-code-braces" @click="navigateTo('/work')" />
            <WindowsButton color="white" :label="$t('about')" icon="mdi-account" @click="navigateTo('/about')" />
            <WindowsButton color="white" :label="messageTranslation" icon="mdi-text-box"
              @click="toggleDialog(dialogs[0])" />
            <WindowsButton color="white" icon="mdi-palette" :label="colorTranslation" @click="toggleDialog(dialogs[1])" />
          </div>
        </div>
      </div>
      <!-- Dialogs -->
      <div class="col-5 col-xs-12 col-sm-11 col-md-10 col-lg-6 col-xl-6 q-px-md placeHolderCol noPointerEvents"></div>
      <div class="col-3 col-xs-11 col-sm-8 col-md-8 col-lg-6 col-xl-5 col-xl-3 q-px-md" :style="dialogContainerStyle"
        style="margin-bottom: 4.5rem">
        <!-- Welcome Message -->
        <!-- When using this approach the position does not reset (with v-if it does) -->
        <div :class="{ hidden: !dialogs[0].isVisible }" class="dialog-container">
          <DialogWelcomeText :zIndex="dialogs[0].zIndex" @mousedown="testMe(0)" @close="closeDialog(dialogs[0])">
          </DialogWelcomeText>
        </div>

        <!-- Color Picker -->
        <div :class="{ hidden: !dialogs[1].isVisible }" class="dialog-container dialog-color-picker ">
          <DialogColorPicker :zIndex="dialogs[1].zIndex" @mousedown="testMe(1)" @close="closeDialog(dialogs[1])" />
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import DialogWelcomeText from "../components/DialogWelcomeText.vue";
import DialogColorPicker from "../components/DialogColorPicker.vue";
import WindowsButton from "../components/WindowsButton.vue";
import { defineComponent, ref, reactive } from "vue";
import { useRouter } from "vue-router";
import { Screen } from "quasar";

export default defineComponent({
  name: "IndexPage",
  components: { DialogWelcomeText, DialogColorPicker, WindowsButton },
  setup() {
    const router = useRouter();
    const dialogs = reactive([
      // Reactive list of dialogs
      {
        name: "welcome",
        zIndex: 1,
        isVisible: ref(true),
      },
      {
        name: "colorPicker",
        zIndex: 2,
        isVisible: ref(false),
      },
    ]);

    const testMe = (number) => {
      console.log("test", number);
      bringToFront(dialogs[number]);
    };

    // Method to handle bringing a dialog to the front
    const bringToFront = (dialog) => {
      // Find the maximum zIndex value among all dialogs
      const maxZIndex = Math.max(...dialogs.map((d) => d.zIndex));
      // Increment the zIndex of the target dialog to bring it to the front
      dialog.zIndex = maxZIndex + 1;
    };

    // Method to handle closing a dialog
    const closeDialog = (dialog) => {
      dialog.isVisible = false;
    };

    // Method to handle opening a dialog
    function openDialog(dialog) {
      dialog.isVisible = true;
      bringToFront(dialog);
    }

    function toggleDialog(dialog) {
      if (dialog.isVisible) {
        // If the dialog is already visible, close it
        closeDialog(dialog);
      } else {
        // If the dialog is not visible, open it
        openDialog(dialog);
      }
    }

    function navigateTo(path) {
      router.push(path);
    }

    return {
      dialogs,
      navigateTo,
      bringToFront,
      closeDialog,
      openDialog,
      toggleDialog,
      testMe
    };
  },
  methods: {
    preventScroll(event) {
      event.preventDefault();
    },
  },
  mounted() {
    document.addEventListener('touchmove', this.preventScroll, { passive: false });
  },
  beforeUnmount() {
    document.removeEventListener('touchmove', this.preventScroll);
  },
  computed: {
    messageTranslation() {
      return this.$t("welcomeMessage");
    },
    colorTranslation() {
      return this.$t("colors");
    },
    // takes care of positioning the dialog container according to screen size
    dialogContainerStyle() {
      const marginValues =
        Screen.xs || Screen.sm || Screen.md
          ? // if screen resolution is xs, sm or md put container to the bottom
          { "margin-left": "0%", "margin-top": "85%" }
          : // if screen resolution is lg or xl put container to the right side
          { "margin-left": "58%", "margin-top": "-13%" };

      return {
        position: "absolute",
        ...marginValues,
      };
    },
  },
});
</script>

<style scoped>
@import "./styles/IndexPage.css";
</style>
