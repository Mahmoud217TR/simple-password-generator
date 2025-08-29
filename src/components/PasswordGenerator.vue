<script setup>
  import RegularButton from './buttons/RegularButton.vue';
  import InputField from './form/InputField.vue';
  import CheckboxField from './form/CheckboxField.vue';
  import emitter from '../eventBus';
</script>

<template>
  <form @submit.prevent="generate">
    <div class="flex flex-col gap-2 mb-6">
      <label for="password" class="text-start">
      Password
    </label>
    <div class="flex w-full border border-neutral-500 rounded-md overflow-hidden">
      <input id='password' class="px-2 py-1 w-full" type="text" v-model="password" disabled>
      <RegularButton class="px-4 py-2 border-x border-neutral-500" type="submit" title="Regenerate">
        <font-awesome-icon icon="fa-solid fa-arrows-rotate" />
      </RegularButton>
      <RegularButton class="px-4 py-2" @click.prevent="copyPassword" title="Copy">
        <font-awesome-icon icon="fa-solid fa-copy" />
      </RegularButton>
    </div>
    </div>
    <div class="flex gap-8 flex-col my-4">
      <div class="grid grid-cols-1 gap-2 justify-center items-center">
        <label for="length" class="text-start">
          Password Length
        </label>
        <InputField id='length' class="" type="number" min="8" max="128" v-model.lazy="length" @change="generate" />
      </div>
      <div class="grid grid-cols-2 gap-4 items-center justify-center">
        <div class="">
          <CheckboxField id='uppercases' class="" type="checkbox" v-model="includeUppercases" @change="generate">
            Uppercase Letters
          </CheckboxField>
        </div>
        <div class="">
          <CheckboxField id='lowercases' class="" type="checkbox" v-model="includeLowercases" @change="generate">
            Lowercase Letters
          </CheckboxField>
        </div>
        <div class="">
          <CheckboxField id='numbers' class="" type="checkbox" v-model="includeNumbers" @change="generate">
            Numbers
          </CheckboxField>
        </div>
        <div class="">
          <CheckboxField id='specials' type="checkbox" v-model="includeSpecial" @change="generate">
            Special characters
          </CheckboxField>
        </div>
      </div>
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-4 items-center justify-around">
        <div class="flex flex-col gap-2">
          <label for="min-numbers">
            Minimum Numbers Count
          </label>
          <InputField id="min-numbers" type="number" :min="includeNumbers ? '1' : '0'" :max="maxNumbers"
            v-model.lazy="minNumbers" :disabled="!includeNumbers" @change="generate" />
        </div>
        <div class="flex flex-col gap-2">
          <label for="min-specials">
            Minimum Special Characters Count
          </label>
          <InputField id='min-specials' type="number" :min="includeSpecial ? '1' : '0'" :max="maxSpecial"
            v-model.lazy="minSpecials" :disabled="!includeSpecial" @change="generate" />
        </div>
      </div>
    </div>
  </form>
</template>

<script>
export default {
  name: "PasswordGenerator",
  data() {
    return {
      password: "",
      length: 8,
      includeUppercases: true,
      includeLowercases: true,
      includeNumbers: true,
      includeSpecial: false,
      minNumbers: 1,
      minSpecials: 0,
    }
  },
  computed: {
    maxNumbers() {
      return this.calcMax(this.minSpecials, this.includeNumbers);
    },
    maxSpecial() {
      return this.calcMax(this.minNumbers, this.includeSpecial);
    }
  },
  watch: {
    length() {
      this.adjustMinSpecials();
      this.adjustMinNumbers();
    },
    minNumbers() {
      this.adjustMinSpecials();
      this.adjustMinNumbers();
    },
    minSpecials() {
      this.adjustMinNumbers();
      this.adjustMinSpecials();
    },
    includeNumbers() {
      this.adjustMinNumbers();
      this.adjustMinSpecials()
    },
    includeSpecial() {
      this.adjustMinSpecials();
      this.adjustMinNumbers();
    }
  },
  methods: {
    generateRandomPassword(
      length,
      useUppercase,
      useLowercase,
      useNumbers,
      useSpecial,
      minNumbers,
      minSpecial,
    ) {

      if (length < 8 || length > 128) {
        throw new Error("Password length must be between 8 and 128");
      }

      const upperChars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
      const lowerChars = "abcdefghijklmnopqrstuvwxyz";
      const numberChars = "0123456789";
      const specialChars = "!@#$%^&*()_+[]{}|;:,.<>?";

      let pool = "";
      if (useUppercase) pool += upperChars;
      if (useLowercase) pool += lowerChars;
      if (useNumbers) pool += numberChars;
      if (useSpecial) pool += specialChars;

      if (!pool) {
        throw new Error("No character sets selected.");
      }

      let password = [];

      for (let i = 0; i < minNumbers; i++) {
        password.push(numberChars[Math.floor(Math.random() * numberChars.length)]);
      }

      for (let i = 0; i < minSpecial; i++) {
        password.push(specialChars[Math.floor(Math.random() * specialChars.length)]);
      }

      while (password.length < length) {
        password.push(pool[Math.floor(Math.random() * pool.length)]);
      }

      for (let i = password.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [password[i], password[j]] = [password[j], password[i]];
      }

      return password.join("");
    },
    generate() {
      this.password = this.generateRandomPassword(
        this.length,
        this.includeUppercases,
        this.includeLowercases,
        this.includeNumbers,
        this.includeSpecial,
        this.minNumbers,
        this.minSpecials,
      )
    },
    adjustMinNumbers() {
      this.minNumbers = this.adjustMinValue(this.minNumbers, this.maxNumbers);
    },
    adjustMinSpecials() {
      this.minSpecials = this.adjustMinValue(this.minSpecials, this.maxSpecial);
    },
    adjustMinValue(minimum, maximum) {
      return Math.max(0, Math.min(Math.max(1, minimum), maximum))
    },
    calcMax(otherMin, included) {
      return Math.max(included, (this.length - otherMin - (this.includeUppercases + this.includeLowercases)) * included);
    },
    async copyPassword() {
      await navigator.clipboard.writeText(this.password);
      emitter.emit("toast", "Password copied to clipboard!");
    }
  },
  mounted() {
    this.generate();
  }
}
</script>

<style>

</style>
