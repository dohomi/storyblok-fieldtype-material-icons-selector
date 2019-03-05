<template>
    <div>
        <div class="uk-form-row">
            <div style="text-align: right" v-if="model.name">
                <a @click="unselect"><small>(<i style="color:darkred" class="uk-icon uk-icon-remove"></i> remove selection)</small></a>
            </div>
            <div class="select select--inline" :class="{'select--open':openSelect}">
                <a class="select__btn ellipsis" @click.stop="openSelect=!openSelect">
                    <template v-if="model.name">
                        <i class="material-icons" v-text="model.id" style="font-size:18px"></i>
                        <small>&nbsp; -- {{model.name}}</small>
                    </template>
                    <span v-else>
                        Please select...
                    </span>
                </a>

                <div class="select__dropdown" :style="{'display':openSelect?'block':'none'}">
                    <div class="select__type-search">
                        <input type="text" class="uk-width-1-1 js-term-input" v-model="search" placeholder="Search">
                    </div>
                    <div style="max-height: 200px; overflow-y: auto; ">
                        <div class="select__opt-box"
                             v-for="item in listItems"
                             :key="item.id">
                            <div @click="selectItem(item)">
                                <i class="material-icons">{{item.id}}</i>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
  // source of material icon list:
  // Credits: https://gist.githubusercontent.com/AmirOfir/daee915574b1ba0d877da90777dc2181/
  import lazyLoadCss from 'lazyload-css'
  import iconObject from './material-icon-list'

  let preparedList = []
  iconObject.categories.forEach(category => {
    preparedList = preparedList.concat(category.icons)
  })

  lazyLoadCss('https://fonts.googleapis.com/icon?family=Material+Icons', 'material-icons')
  export default {
    mixins: [window.Storyblok.plugin],
    data () {
      return {
        search: '',
        openSelect: false
      }
    },
    methods: {
      unselect(){
        this.$set(this.model, 'name', null)
        this.$set(this.model, 'id', null)
      },
      selectItem (item) {
        this.$set(this.model, 'name', item.ligature)
        this.$set(this.model, 'id', item.id)
        this.openSelect = false
      },
      initWith () {
        return {
          // needs to be equal to your storyblok plugin name
          plugin: 'material-icons-selector'
        }
      },
      pluginCreated () {
        // eslint-disable-next-line
        console.log('View source and customize: https://github.com/storyblok/storyblok-fieldtype')
      }
    },
    computed: {
      listItems () {
        if (this.search) {
          return preparedList.filter(item => item.name.includes(this.search))
        }
        return preparedList
      }
    },
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
