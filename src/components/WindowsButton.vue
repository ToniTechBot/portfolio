<template>
  <q-btn square no-caps flat class="windows-button" @click="executeProgram" @mouseover="isHovered = true"
    @mouseout="isHovered = false">
    <div class="button-content">
      <q-icon :name="icon" class="icon" :color="color" size="2rem" />
      <div class="text" v-if="label">
        {{ label }}
      </div>
    </div>
  </q-btn>
</template>

<script>
export default {
  props: {
    icon: {
      type: String,
      default: "mdi-windows",
    },
    color: {
      type: String,
      default: "red",
    },
    label: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      isHovered: false,
    };
  },
  methods: {
    executeProgram() {
      // Code to execute when the button is clicked
    },
  },
};
</script>

<style scoped>
.windows-button {
  pointer-events: auto;
  padding: 10px;
  border-radius: 1px;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s;
  border: 1px solid rgba(0, 0, 0, 0);
  text-shadow: rgb(77, 73, 129) 1px 1px 2px;
}

.windows-button:hover {
  background-color: rgba(0, 120, 215, 0.3);
  border: 1px solid rgba(0, 0, 0, 0.1);
}

.button-content {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.text {
  margin-top: 8px;
  font-size: 0.8rem;
  color: white;
  text-shadow: rgb(62, 58, 102) 1px 1px 3px;
}
</style>
