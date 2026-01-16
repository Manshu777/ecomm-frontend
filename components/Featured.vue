<template>
  <div class="area home-section">
    <template v-if="itemList && itemList.length">

      <div class="flex sided title">
        <h4>{{ $t('filter.featured', {title: title}) }}</h4>
        <nuxt-link
          class="link"
          to="/categories"
        >
          {{ $t('featured.showAll') }}
        </nuxt-link>
      </div>

      <div class="area-content">
          <image-slider
            :image-count="itemList.length"
            :per-view="9"
            :gap="15"
            :responsive="[7, 5, 4, 3, 2]"
          >
            <template v-slot:content>
              <li
                v-for="(value, index) in itemList"
                :key="index"
                class="center-text"
              >
                <sub-category-tile
                  :category="value.category"
                  :sub-category="value"
                />
              </li>


            </template>
          </image-slider>


          <ProductBanner :itemList="featuredItems" />
      </div>
    </template>
  </div>
</template>

<script>
  import util from '~/mixin/util'
  import ImageSlider from '~/components/ImageSlider'
  import LazyImage from '~/components/LazyImage'
  import SubCategoryTile from "./SubCategoryTile";

  import ProductBanner from './ProductBanner.vue';

    export default {
        name: 'Featured',
        data() {
            return {
            }
        },
        watch: {
        },
        props: {
          title: {
            type: String,
            default: ''
          },
          itemList: {
            type: Array,
            default(){
              return []
            }
          }
        },
        components: {
          SubCategoryTile,
          ImageSlider,
          LazyImage,
          ProductBanner
        },
        computed: {
        },
        mixins: [util],
        methods: {
        },
        created() {
        },
        mounted() {
          console.log('Featured Titled2:', this.itemList);
  console.log('Featured Item Listd2:', this.itemList);
        }
    }
</script>


<style scoped>
.home-section {
  display: flex;
  gap: 20px;
}

.area-content {
  display: flex;
  justify-content: center;

}

.card_data {
  width: 425px;
  padding: 20px;
  background-color: #fff;
}

.product-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  list-style-type: none;
  padding: 0;
}

.product_name{
  margin-top: 2px;
  font-size: 14px;
}

.category_titles h5 {
  font-size: 1.4rem;
  line-height: 1.8rem;
  margin-bottom: 10px;
}

.category_product_section {

  display: flex;
  justify-content: center;
  gap: 15px;
  margin-top: 10px;
  margin-bottom: 10px;

}

.prduct_img_category {
  width: 160px;


}
</style>
