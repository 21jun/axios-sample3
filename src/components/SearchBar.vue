<template>
  <v-form>
    <v-container>
      <v-layout row wrap>

        <v-flex xs12 sm6>
          <v-text-field v-model="qurryURL" label="검색어 입력" solo-inverted></v-text-field>
        </v-flex>
        <search-button/>
      </v-layout>
    </v-container>
  </v-form>
</template>

<script>
  import SearchButton from '@/components/SearchButton'
  export default {
    data: () => ({
      qurryURL: ""
    }),
    created() {
        this.$EventBus.$on('search-button', ()=>{
            this.$EventBus.$emit('search', this.qurryURL)
            console.log('search bar event passed')
        });
    },
    methods:{

    },
    components: {
      SearchButton
    }
  }

</script>
