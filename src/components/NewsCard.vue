<template>
  <v-data-table :headers="headers" :items="posts" class="elevation-1">
    <template slot="items" slot-scope="props">
      <td>{{ props.item.title }}</td>
      <td class="text-xs-right"><a :href="props.item.link">{{ props.item.title }}</a> </td>
      <td class="text-xs-right">{{ props.item.date }}</td>
    </template>
    <template slot="pageText" slot-scope="props">
      Lignes {{ props.pageStart }} - {{ props.pageStop }} de {{ props.itemsLength }}
    </template>
  </v-data-table>
</template>
<script>
  import axios from 'axios'
  import cleanStr from '../add_js/cleanString'
  let clientId = "fk2b6H7p_xkrQtrtoKQP";
  let clientSecret = "GijlgkzUzM";

  export default {
    data() {
      return {
        ueryURL: "nexon",
        posts: [],
        selected: [2],
        headers: [{
            text: 'News',
            //align: 'left',
            sortable: false,
            value: 'name'
          },
          {
            text: '가나다 순',
            value: 'title'
          },
          {
            text: '날짜 순',
            value: 'date'
          }
        ],
      }
    },
    created() {
      this.$EventBus.$on('search', this.search)
    },
    mounted() {
      this.searchNaverAPI();
    },
    methods: {
      toggle(index) {
        const i = this.selected.indexOf(index)

        if (i > -1) {
          this.selected.splice(i, 1)
        } else {
          this.selected.push(index)
        }
      },
      search(url) {
        console.log(url)
        this.queryURL = url;
        this.searchNaverAPI();
      },
      searchNaverAPI: function () {
        axios({
          method: "get",
          url: "/naversearch",
          params: {
            query: this.queryURL,
            display: "100"

          },
          crossDomain: true,
          headers: {
            "X-Naver-Client-Id": clientId,
            "X-Naver-Client-Secret": clientSecret
          }
        }).then((result) => {
          console.log(result);
          let news = result.data.items;

          this.posts = [];
          for (let i in news) {
            var tmp = {
              title: cleanStr(news[i].title, "title"),
              link: news[i].originallink,
              date: news[i].pubDate
            };
            this.posts.push(tmp)
          }

        }).catch((ex) => {
          console.log("ERROR")
        })
      }
    }
  }

</script>
<style lang="">

</style>
