<template>
  <div class="vmix-audio-mixer">
    <div class="vmix-audio-mixer__vmix-url">
      <label for="vmix-url">vMix URL</label>
      <input type="text" v-model="vMixUrl" id="vmix-url" />
      <button
        @click="setvMixUrl()"
      >Connect</button>
    </div>
    <div v-if="vMixUrlSet" class="vmix-audio-mixer__inputs">
      <AudioInput
        v-for="audioInput in audioInputs"
        :key="audioInput.key"
        :audio-input="audioInput"
        :on-set-volume="(volume) => setVolume(volume, audioInput.key)"
      />
    </div>
  </div>
</template>

<script>
import AudioInput from "./AudioInput";

export default {
  name: "AudioMixer",
  components: {
    AudioInput,
  },
  data() {
    return {
      vMixUrlSet: false,
      vMixUrl: "",
      pollingInterval: null,
      currentText: null,
      currentXml: null,
      audioInputs: [],
    };
  },
  methods: {
    async setvMixUrl() {
      if (this.vMixUrlSet) {
        return;
      }
      this.vMixUrlSet = true;

      await this.fetchAudioInputs();
      this.startPolling();

      console.log(this.audioInputs);

      console.log("vMix URL set");
    },
    async fetchAudioInputs() {
      const xml = await this.getInputsXml();

      const inputs = [...xml.querySelectorAll("vmix inputs input")];
      const audioInputXmls = inputs.filter((input) => input.hasAttribute("meterF1"));

      this.audioInputs = this.parseXml(audioInputXmls);
    },
    async getInputsXml() {
      const response = await fetch(`${this.vMixUrl}/API`);
      const text = await response.text();
      if (text === this.currentText) return this.currentXml;

      this.currentText = text;
      this.currentXml = new window.DOMParser().parseFromString(text, "text/xml");

      return this.currentXml;
    },
    startPolling() {
      const TIMEOUT = 1;
      this.pollingInterval = setInterval(() => {
        // this.fetchAudioInputs();
        // console.log("Polling ...");
      }, TIMEOUT * 1000);
    },
    async setVolume(volume, inputKey) {
      console.log("CHANGE", volume, inputKey);
      const url = `${this.vMixUrl}/API/?Function=SetVolumeFade&Input=${inputKey}&Value=${volume},100`;
      console.log(url);
      await fetch(url);
    },
    parseXml(inputXml) {
      const _parseXml = (inputXml) => {
        const xmlAttributes = inputXml.getAttributeNames();

        const parsed = {};
        xmlAttributes.forEach((attr) => {
          const attrValue = inputXml.getAttribute(attr);
          parsed[attr] = attrValue;
        });

        return parsed;
      };

      if (Array.isArray(inputXml)) {
        return inputXml.map((xml) => _parseXml(xml));
      }

      return _parseXml(inputXml);
    },
  },
};
</script>

<style scoped>
.vmix-audio-mixer__vmix-url {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 20px;
}
.vmix-audio-mixer__vmix-url label {
  color: #d1d1d1;
  margin-right: 0.5rem;
}
.vmix-audio-mixer__inputs {
  display: flex;
  justify-content: center;
}
</style>
