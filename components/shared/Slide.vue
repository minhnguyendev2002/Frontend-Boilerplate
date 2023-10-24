<template>
    <div class="new-carousel mt-5 px-5">
        <a-empty v-if="news.length === 0" class="!mt-10" />
        <VueSlickCarousel
            v-else
            key="newSlide"
            ref="SliderCarousel"
            v-bind="settings"
        >
            <div v-for="_new in news" :key="_new.id">
                <div class="w-full h-full relative cursor-pointer">
                    <div v-if="loading">
                        <Loading />
                    </div>
                    <div v-else>
                        <nuxt-link :to="`/mua/${_new._id}`" class="shadow rounded">
                            <div class="flex flex-col h-full">
                                <div class="w-full overflow-hidden h-[150px]">
                                    <img class="rounded object-cover w-full h-full" :src="_new.medias && _new.medias[0].source" alt="">
                                </div>
                                <div class="p-2 flex flex-col flex-1">
                                    <h3 class="font-semibold text-sm hover:text-second-100 transition-all duration-300 uppercase line-clamp-2">
                                        {{ _new.title }}
                                    </h3>
                                    <div class="flex gap-y-2 gap-x-5 mb-2 flex-wrap">
                                        <p class="font-semibold text-[red] text-lg mb-0">
                                            <span>
                                                <span v-if="_new.category === 'buy'">{{ convertPrice(+_new.purchasePrice) }}</span>
                                                <span v-if="_new.category === 'rent'">{{ convertPrice(+_new.rentPrice) }}</span>
                                                <span class="px-2">·</span> {{ _new.infor.area }}m</span>
                                        </p>
                                        <span><i class="fas fa-map-pin mr-2" />{{ _new.district && _new.district.name }}{{ _new.district && _new.district.name ? ', ': '' }} {{ _new.province && _new.province.name }}</span>
                                    </div>
                                    <div class="text-sm mt-auto">
                                        Ngày đăng: {{ _new.createdAt | dateFormat }}
                                    </div>
                                </div>
                            </div>
                        </nuxt-link>
                    </div>
                </div>
            </div>
            <template #prevArrow>
                <div slot="prev-arrow" class="absolute !top-[110%] !left-[30%] sm:!left-[35%] lg:!left-[44%] !flex !flex-col !items-center !justify-center !w-12 !h-12 rounded-full !text-center !bg-prim-900 hover:!text-prim-900 !border-[2px] !border-solid !border-prim-900 !text-white duration-300 hover:!bg-white">
                    <i class="fas fa-chevron-left  text-3xl" />
                </div>
            </template>
            <template #nextArrow>
                <div slot="next-arrow" class="absolute !top-[110%] !right-[30%] sm:!right-[35%] lg:!right-[44%] !flex !flex-col !items-center !justify-center !w-12 !h-12 rounded-full !text-center !bg-prim-900 hover:!text-prim-900 !border-[2px] !border-solid !border-prim-900 !text-white duration-300 hover:!bg-white">
                    <i class="fas fa-chevron-right  text-3xl" />
                </div>
            </template>

            <template #customPaging>
                <div slot="custom-paging">
                    <span class="block mx-auto w-20 h-1 rounded-full bg-white duration-300" />
                </div>
            </template>
        </VueSlickCarousel>
    </div>
</template>

<script>
    import VueSlickCarousel from 'vue-slick-carousel';
    import 'vue-slick-carousel/dist/vue-slick-carousel-theme.css';
    import 'vue-slick-carousel/dist/vue-slick-carousel.css';
    import Loading from '@/components/shared/LoadingSecond.vue';
    import { convertPrice } from '@/utils/data';

    export default {
        components: {
            VueSlickCarousel,
            Loading,
        },

        props: {
            news: {
                type: Array,
                default: () => [],
            },
        },

        data() {
            return {
                loading: false,
                settings: {
                    speed: 500,
                    dots: false,
                    infinite: true,
                    arrows: true,
                    slidesToShow: 3,
                    slidesToScroll: 1,
                    autoplay: true,
                    responsive: [
                        {
                            breakpoint: 1280,
                            settings: {
                                slidesToShow: 3,
                            },
                        },
                        {
                            breakpoint: 1279,
                            settings: {
                                arrows: true,
                                slidesToShow: 2,
                            },
                        },
                        {
                            breakpoint: 768,
                            settings: {
                                arrows: true,
                                slidesToShow: 1,
                            },
                        },
                    ],
                },
            };
        },

        watch: {
        },

        methods: {
            convertPrice,
        },
    };
</script>

<style lang="scss">
    .new-carousel {
        .slick-prev:before, .slick-next:before {
            content: '' !important;
        }
        .slick-list {
            overflow: hidden;
            padding: 0px 0px !important;
        }
        .slick-slide {
            float: left !important;
            padding: 0px 5px !important;
        }
        .slick-slider {
            margin: 0px -20px !important;
        }
        .slick-dots {
            bottom: -6px;
            .slick-active {
                span {
                    @apply w-20 bg-black-100 relative;
                }
            }
            li {
                @apply w-20 mx-1;
            }
        }
        p {
            @apply mb-0
        }
    }
</style>
