<template>
  <div v-if="license">
    <ul class="margin-vertical-small">
      <template v-for="(li, index) in splitLicenses">
        <li
          :class="{
            ['margin-vertical-small']: true,
            ['is-flex']: true,
          }"
          :key="index"
        >
          <i
            :class="{
              icon: true,
              ['has-text-black']: true,
              ['has-background-white']: true,
              ['is-size-4']: true,
              ['margin-right-small']: true,
              [`cc-${getLicenseIcon(li)}`]: true,
            }"
            :alt="`${li.toUpperCase()}`"
          />
          <p>{{ getLicenseDescription(li) }}</p>
        </li>
      </template>
    </ul>
  </div>
</template>

<script>
const APItoIconNameMap = {
  by: 'by',
  nc: 'nc',
  nd: 'nd',
  sa: 'sa',
  cc0: 'zero',
  pdm: 'pd',
}

const LicenseTermDescriptions = {
  by: 'Credit the creator.',
  nc: 'Noncommercial uses only.',
  nd: 'No derivatives or adaptations permitted.',
  sa: 'Share adaptations under the same terms.',
  cc0: 'This work has been marked as dedicated to the public domain.',
  pdm: 'This work is marked as being in the public domain.',
}

const LicenseExplanations = {
  name: 'license-explanations',
  components: {},
  props: {
    license: '',
  },
  computed: {
    splitLicenses() {
      return this.$props.license.split('-')
    },
  },
  methods: {
    getLicenseIcon(licenseTerm) {
      return APItoIconNameMap[licenseTerm]
    },
    getLicenseDescription(licenseTerm) {
      return LicenseTermDescriptions[licenseTerm]
    },
  },
}

export default LicenseExplanations
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped></style>
