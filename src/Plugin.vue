<template>
    <div>
        <div class="uk-form-row">
                <h3>hallo</h3>
        </div>
    </div>
</template>

<script>
  export default {
    mixins: [window.Storyblok.plugin],
    data () {
      return {
        search: '',
        openSelect: false
      }
    },
    methods: {
      initWith () {
        return {
          // needs to be equal to your storyblok plugin name
          plugin: 'material-icons-selection',
          title: '',
          description: ''
        }
      },
      pluginCreated () {
        // eslint-disable-next-line
        console.log('View source and customize: https://github.com/storyblok/storyblok-fieldtype')
      }
    },
    // computed: {
    //   listSvgItems () {
    //     return []
    //   }
    // },
    watch: {
      'model': {
        handler: function (value) {
          this.$emit('changed-model', value)
        },
        deep: true
      }
    }
  }
</script>
