<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <h1 class="display-1 font-weight-bold mb-3">
          {{message}}
        </h1>
      </v-col>

      <v-col cols="12">
        <v-btn class="mx-2" color="primary" @click="$emit('onskip')" :disabled="paused">
          <v-icon left>
            mdi-check-circle-outline
          </v-icon>
          Done ({{timeout}})
        </v-btn>
        <v-btn class="mx-2" color="primary" @click="paused ? $emit('oncontinue') : $emit('onpause')">
          <v-icon left>
            {{ paused ? 'mdi-timer-outline' : 'mdi-pause-circle-outline'}}
          </v-icon>
          {{ paused ? 'Continue' : 'Pause'}}
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'BreakReminder',

    props: {
        timeout: Number,
        message: String,
        paused: Boolean
    }
  }
</script>
