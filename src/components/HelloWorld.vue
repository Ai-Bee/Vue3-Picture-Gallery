<template>
  <div class="hello">
    <div>
      <div class="imgContainer">
        <template :key="item.id" v-for="item in singlePage">
          <img :alt="item.author" :src="item.download_url" />
        </template>
      </div>
      <div class="pagination" v-show="allImages.length !== 0">
        <a v-if="currentPage !== 1" @click.prevent="prevPage">&laquo;</a>
        <template :key="num" v-for="num in numberOfPages">
          <a
            @click.prevent="goToPage(num)"
            :class="{ active: num === currentPage }"
            >{{ num }}</a
          >
        </template>
        <a v-show="currentPage !== allImages" @click.prevent="nextPage"
          >&raquo;</a
        >
      </div>
    </div>
  </div>
</template>

<script>
import { ref, watch, computed } from "vue";

export default {
  props: {
    allImages: {
      default: () => [],
      type: Array,
    },
  },
  setup(props) {
    let currentPage = ref(1);
    let singlePage = ref([]);
    let itemsPerPage = ref(6);
    const numberOfPages = computed(() => {
      return Math.ceil(props.allImages.length / itemsPerPage.value);
    });
    const theList = computed(() => {
      return props.allImages;
    });
    function goToPage(page) {
      // eslint-disable-next-line prettier/prettier
      const first = ( page * itemsPerPage.value ) - itemsPerPage.value;
      const last = page * itemsPerPage.value;
      currentPage.value = page;
      singlePage.value = props.allImages.slice(first, last);
    }
    function nextPage() {
      goToPage(currentPage.value);
    }
    function prevPage() {
      goToPage(currentPage.value);
    }
    watch(theList, () => {
      goToPage(1);
    });

    return {
      goToPage,
      itemsPerPage,
      numberOfPages,
      nextPage,
      prevPage,
      singlePage,
      currentPage,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.hello {
  display: flex;
  align-content: center;
  justify-content: center;
  position: relative;
  button {
    outline: none;
    background-color: aquamarine;
    padding: 20px 10px;
    transition: all 1s ease-in;
    border: none;
    position: absolute;
    top: 9px;
    left: 44%;
  }
  button:hover {
    border: 2px solid gray;
    background-color: aqua;
  }
  h2 {
    background-color: aquamarine;
  }
  div.imgContainer {
    display: flex;
    align-content: center;
    justify-content: center;
    position: relative;
    margin-top: 1%;
    background: rgb(71, 67, 145);
    background: linear-gradient(
      0deg,
      rgba(71, 67, 145, 1) 0%,
      rgba(200, 198, 225, 1) 38%,
      rgba(218, 216, 235, 1) 67%
    );
    img {
      max-width: 60%;
    }
  }
  .pagination {
    display: inline-block;
    margin-top: 10px;
    margin-left: 20px;
    max-width: 79%;
    position: fixed;
    bottom: 10px;
    justify-content: center;
  }
  .pagination a {
    color: black;
    float: left;
    padding: 8px 16px;
    text-decoration: none;
    transition: background-color 0.3s;
  }
  .pagination a.active {
    background-color: #ff013c;
    color: white;
    border-radius: 5px;
  }
  .pagination a:hover:not(.active) {
    background-color: #ddd;
    border-radius: 5px;
  }
}
</style>
