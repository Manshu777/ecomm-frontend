<template>
    <header :class="{ 'no-banner': (topBannerLoaded && isTopBannerClosed) || !isPublic }">
     

        <div class="top-wrapper">
            <div class="container-fluid">

                <div ref="headerWrapper" class="wrap flex sided ">
                    <div class="left-side wrap flex gap-10 d-none-mb">
                        <dropdown v-if="Object.keys(languages).length > 1" :selected-key="currentLanguage.code"
                            :options="languages" key-name="name" class="lang-dropdown" @clicked="selectedLanguage" />

                        <a :href="`mailto:${email}`" class="flex gap-5">
                            <i class="icon email-icon" />
                            <span>
                                <span class="hide-md">{{ $t('home.mail') }}</span>
                                {{ email }}
                            </span>
                        </a>

                        <template v-if="phone">
                            <span>|</span>
                            <a :href="`tel:${phone}`" class="flex gap-5">
                                <i class="icon phone-icon" />
                                <span><span class="hide-md">{{ $t('home.helpline') }}</span> {{ phone }}</span>
                            </a>
                        </template>
                    </div>

                    <div class="flex right-side text-upper d-none-mb">
                        <div class="flex gap-5" v-if="!isLoggedIn">
                            <template v-if="sellerSignUp">
                                <nuxt-link to="/seller/register" class="flex gap-5">
                                    {{ $t('date.bav') }}
                                </nuxt-link>
                                <span>|</span>
                            </template>

                            <nuxt-link to="/login" class="flex gap-5">
                                <i class="icon login-icon" />
                                {{ $t('header.login') }}
                            </nuxt-link>
                            <span>|</span>
                            <nuxt-link to="/register" class="flex gap-5">
                                <i class="icon register-icon" />
                                {{ $t('header.register') }}
                            </nuxt-link>
                        </div>

                        <nuxt-link v-else to="/user/profile" class="flex gap-5">
                            <i class="icon user-icon" />
                            {{ $t('header.profile') }}
                        </nuxt-link>
                    </div>
                </div>
            </div>
        </div>

        <div class="header-sticky" :class="{ sticky: headerSticky }">
            <div class="container-fluid flex pos-rel">
                <div class="left-area">

                    <span class="d-none-lg" @click="toggleSidebar">
                        <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 16 16">
                            <path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"
                                stroke-width="1.5" d="M2.75 12.25h10.5m-10.5-4h10.5m-10.5-4h10.5" />
                        </svg>
                    </span>

                    <nuxt-link to="/" class="logo">
                        <img :src="imageURL({ 'image': site_setting.header_logo })" :alt="$t('footer.siteLogo')"
                            height="40" width="139">
                    </nuxt-link>
                </div>

                <div class="main_div_locations">
                    <i class="icon ic-wid location-icon " />
                    <div class="main_div_location">

                        <span class="text-white" v-if="loading">
                            <img class="img_loder" src="~assets/images/loader.gif" alt="Discover Nuxt 3" />
                        </span>
                        <div class="user_dynmic_add" v-else>
                            <span>Delivering to</span>

                            <span> {{ district }} {{ picncode }}</span>

                        </div>

                    </div>
                </div>
                   
                <div class="topBar-serach-bar ">
                    <span class="menu-bar  hide-on-desktop"  @click="toggleSidebar"   :class="{ 'menu-none': !showMenuButton }" >
                        <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 16 16">
                            <path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"
                                stroke-width="1.5" d="M2.75 12.25h10.5m-10.5-4h10.5m-10.5-4h10.5" />
                        </svg>
                    </span>


                
                    <form class="search-input grow" @submit.prevent="search">
                    <input @focus="openSearchPopup" @blur="blurSearchInput" type="text"
                        :placeholder="$t('header.searchHere')" v-model="searchedText">
                    <button aria-label="submit" type="submit" class="flex">
                        <i class="icon search-icon" />
                    </button>
                    



                    <search-popup v-if="searchPopup" :searched-text="searchedText" @close="closeSearchPopup" />
                    
                </form>
                <nuxt-link to="/cart" class="cart-btn flex pos-rel h-40x gap-5 hide-on-desktop" :class="{ 'cart-none': !showCartButton }">
                        <span v-if="cartCount" class="cart-badge">
                            {{ cartCount }}
                        </span>
                        <i class="icon cart-icon icon-cart " />
                        <!-- <span class="title">{{ $t('header.cart') }}</span> -->
                </nuxt-link>
                </div>

                

                <div class="right-area flex gap-15 right">
                    <div class="pos-rel" v-outside-click="closeDropdown">
                        <button aria-label="submit" class="flex gap-10" @click="dropdown = !dropdown">
                            {{ $t('header.account') }}
                            <i class="icon arrow-down " />
                        </button>
                        <div class="dropdown" :class="{ active: dropdown }">
                            <nuxt-link to="/user/orders">
                                {{ $t('header.orders') }}
                            </nuxt-link>
                            <nuxt-link to="/user/wishlists">
                                {{ $t('header.wishList') }}
                            </nuxt-link>
                            <nuxt-link to="/user/compared">
                                {{ $t('header.comparedList') }}
                            </nuxt-link>
                            <nuxt-link to="/user/vouchers">
                                {{ $t('header.vouchers') }}
                            </nuxt-link>
                            <button aria-label="Logout" v-show="isLoggedIn" class="clear-btn"
                                @click.prevent="loggingOut">
                                {{ $t('header.logout') }}
                            </button>
                        </div>
                    </div>
                    <nuxt-link to="/cart" class="cart-btn flex pos-rel h-40x gap-5">
                        <span v-if="cartCount" class="cart-badge">
                            {{ cartCount }}
                        </span>
                        <i class="icon cart-icon " />
                        <span class="title">{{ $t('header.cart') }}</span>
                    </nuxt-link>
                </div>
            </div>


            <div class="bottom-area text-nowrap">
                <div class="container-fluid">


                    <div class="flex  bg-color-amaz">


                        <a class="text-white hover:text-gray-200 mr-4 flex items-center" @click="toggleSidebar">
                            <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 16 16">
                                <path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"
                                    stroke-width="1.5" d="M2.75 12.25h10.5m-10.5-4h10.5m-10.5-4h10.5" />
                            </svg>
                            <span class="ml-4">All</span>
                        </a>
                        <div>
                            <a class="text-white hover:text-gray-200 mr-4 flex items-center">

                                <span class="ml-4">Fresh</span>
                            </a>

                        </div>
                        <div>
                            <a class="text-white hover:text-gray-200 mr-4 flex items-center">

                                <span class="ml-4">Amazon Tv</span>
                            </a>

                        </div>

                        <div>
                            <a class="text-white hover:text-gray-200 mr-4 flex items-center">

                                <span class="ml-4">Best Seller </span>
                            </a>

                        </div>

                        <div>
                            <a class="text-white hover:text-gray-200 mr-4 flex items-center">

                                <span class="ml-4">Today deal's</span>
                            </a>

                        </div>

                        <div>
                            <a class="text-white hover:text-gray-200 mr-4 flex items-center">

                                <span class="ml-4">Customer Services</span>
                            </a>

                        </div>

                        <div>
                            <a class="text-white hover:text-gray-200 mr-4 flex items-center">

                                <span class="ml-4">New Release </span>
                            </a>

                        </div>

                        <div>
                            <a class="text-white hover:text-gray-200 mr-4 flex items-center">

                                <span class="ml-4"> Home & Kitchen </span>
                            </a>

                        </div>
                        <div>
                            <a class="text-white hover:text-gray-200 mr-4 flex items-center">

                                <span class="ml-4">Electronics </span>
                            </a>

                        </div>

                        <div>
                            <a class="text-white hover:text-gray-200 mr-4 flex items-center">

                                <span class="ml-4">Laptops</span>
                            </a>

                        </div>

                        <div>
                            <a class="text-white hover:text-gray-200 mr-4 flex items-center">

                                <span class="ml-4">Laptops</span>
                            </a>

                        </div>
                        <div>
                            <a class="text-white hover:text-gray-200 mr-4 flex items-center">

                                <span class="ml-4">Laptops</span>
                            </a>

                        </div>






                        <!-- <div>
                          <nuxt-link v-for="(item, index) in headerLeft" :key="index" :to="getUrl(item)">
                              <span>
                                  {{ getTitle(item) }}
                              </span>
                          </nuxt-link>
                      </div>
                      <div> -->

                        <!-- <nuxt-link v-for="(item, index) in headerRight" :key="index" :to="getUrl(item)">
                              <span>
                                  {{ getTitle(item) }}
                              </span>
                          </nuxt-link> -->





                    </div>
                </div>
            </div>
        </div>





        <transition name="sidebar-home-fade">
            <aside class="sidebar-home" v-if="sidebarOpen">

                <div class="top_sidebarss">
                    <span class="flex justify-between items-center bg-amaz wh">



                        <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24">
                            <path fill="currentColor"
                                d="M5.85 17.1q1.275-.975 2.85-1.537T12 15t3.3.563t2.85 1.537q.875-1.025 1.363-2.325T20 12q0-3.325-2.337-5.663T12 4T6.337 6.338T4 12q0 1.475.488 2.775T5.85 17.1M12 13q-1.475 0-2.488-1.012T8.5 9.5t1.013-2.488T12 6t2.488 1.013T15.5 9.5t-1.012 2.488T12 13m0 9q-2.075 0-3.9-.788t-3.175-2.137T2.788 15.9T2 12t.788-3.9t2.137-3.175T8.1 2.788T12 2t3.9.788t3.175 2.137T21.213 8.1T22 12t-.788 3.9t-2.137 3.175t-3.175 2.138T12 22" />
                        </svg>
                        <span class="top_sidebars_head">Hello, Sign in</span>
                    </span>


                    <button @click="toggleSidebar">
                        <svg class="svg_cross" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                            <path fill="#ffffff" fill-rule="evenodd"
                                d="M6.293 6.293a1 1 0 0 1 1.414 0L12 10.586l4.293-4.293a1 1 0 1 1 1.414 1.414L13.414 12l4.293 4.293a1 1 0 0 1-1.414 1.414L12 13.414l-4.293 4.293a1 1 0 1 1-1.414-1.414L10.586 12 6.293 7.707a1 1 0 0 1 0-1.414z" />
                        </svg>

                    </button>






                </div>




                <!-- <ul>
      <li v-for="(item, index) in sidebarItems" :key="index">
        <router-link :to="item.route">{{ item.label }}</router-link>
      </li>
    </ul> -->

                <ul class="sidebarContent">
                    <li class="heading_sidebar">
                        Trending
                    </li>
                    <li class="heading_sidebar_child ">
                        <a href="#" class="heading_sidebar_anchor">
                            Best Sellers
                        </a>
                    </li>
                    <li class="heading_sidebar_child ">
                        <a href="#" class="heading_sidebar_anchor">
                            New Release
                        </a>
                    </li>
                    <li class="heading_sidebar_child ">
                        <a href="#" class="heading_sidebar_anchor">
                            Movers and Shakers
                        </a>
                    </li>
                </ul>

                <ul class="sidebarContent">
                    <li class="heading_sidebar">
                        Digital Content and Devices
                    </li>
                    <li class="heading_sidebar_child">
                        <a href="#" class="heading_sidebar_anchor_two">
                            Best Sellers
                            <span class="">
                                <svg class="text-sm " xmlns="http://www.w3.org/2000/svg" width="18" height="18"
                                    viewBox="0 0 24 24">
                                    <path fill="currentColor"
                                        d="M8.025 22L6.25 20.225L14.475 12L6.25 3.775L8.025 2l10 10z" />
                                </svg>
                            </span>
                        </a>
                    </li>
                    <li class="heading_sidebar_child">
                        <a href="#" class="heading_sidebar_anchor_two">
                            Fire Tv
                            <span class="">
                                <svg class="text-sm " xmlns="http://www.w3.org/2000/svg" width="18" height="18"
                                    viewBox="0 0 24 24">
                                    <path fill="currentColor"
                                        d="M8.025 22L6.25 20.225L14.475 12L6.25 3.775L8.025 2l10 10z" />
                                </svg>
                            </span>
                        </a>
                    </li>
                    <li class="heading_sidebar_child">
                        <a href="#" class="heading_sidebar_anchor_two">
                            Alexa & Enco
                            <span class="">
                                <svg class="text-sm " xmlns="http://www.w3.org/2000/svg" width="18" height="18"
                                    viewBox="0 0 24 24">
                                    <path fill="currentColor"
                                        d="M8.025 22L6.25 20.225L14.475 12L6.25 3.775L8.025 2l10 10z" />
                                </svg>
                            </span>
                        </a>
                    </li>
                    <li class="heading_sidebar_child">
                        <a href="#" class="heading_sidebar_anchor_two">
                            Kindle & E-Readers
                            <span class="">
                                <svg class="text-sm " xmlns="http://www.w3.org/2000/svg" width="18" height="18"
                                    viewBox="0 0 24 24">
                                    <path fill="currentColor"
                                        d="M8.025 22L6.25 20.225L14.475 12L6.25 3.775L8.025 2l10 10z" />
                                </svg>
                            </span>
                        </a>
                    </li>
                    <li class="heading_sidebar_child">
                        <a href="#" class="heading_sidebar_anchor_two">
                            Audioable Audiobooks
                            <span class="">
                                <svg class="text-sm " xmlns="http://www.w3.org/2000/svg" width="18" height="18"
                                    viewBox="0 0 24 24">
                                    <path fill="currentColor"
                                        d="M8.025 22L6.25 20.225L14.475 12L6.25 3.775L8.025 2l10 10z" />
                                </svg>
                            </span>
                        </a>
                    </li>



                </ul>


                <ul class="sidebarContent">
                    <li class="heading_sidebar">
                        Shop by Category
                    </li>
                    <li class="heading_sidebar_child">
                        <a href="#" class="heading_sidebar_anchor_two">
                            Best Sellers
                            <span class="">
                                <svg class="text-sm " xmlns="http://www.w3.org/2000/svg" width="18" height="18"
                                    viewBox="0 0 24 24">
                                    <path fill="currentColor"
                                        d="M8.025 22L6.25 20.225L14.475 12L6.25 3.775L8.025 2l10 10z" />
                                </svg>
                            </span>
                        </a>
                    </li>
                    <li class="heading_sidebar_child">
                        <a href="#" class="heading_sidebar_anchor_two">
                            Fire Tv
                            <span class="">
                                <svg class="text-sm " xmlns="http://www.w3.org/2000/svg" width="18" height="18"
                                    viewBox="0 0 24 24">
                                    <path fill="currentColor"
                                        d="M8.025 22L6.25 20.225L14.475 12L6.25 3.775L8.025 2l10 10z" />
                                </svg>
                            </span>
                        </a>
                    </li>
                    <li class="heading_sidebar_child">
                        <a href="#" class="heading_sidebar_anchor_two">
                            Alexa & Enco
                            <span class="">
                                <svg class="text-sm " xmlns="http://www.w3.org/2000/svg" width="18" height="18"
                                    viewBox="0 0 24 24">
                                    <path fill="currentColor"
                                        d="M8.025 22L6.25 20.225L14.475 12L6.25 3.775L8.025 2l10 10z" />
                                </svg>
                            </span>
                        </a>
                    </li>
                    <li class="heading_sidebar_child">
                        <a href="#" class="heading_sidebar_anchor_two">
                            Kindle & E-Readers
                            <span class="">
                                <svg class="text-sm " xmlns="http://www.w3.org/2000/svg" width="18" height="18"
                                    viewBox="0 0 24 24">
                                    <path fill="currentColor"
                                        d="M8.025 22L6.25 20.225L14.475 12L6.25 3.775L8.025 2l10 10z" />
                                </svg>
                            </span>
                        </a>
                    </li>
                    <li class="heading_sidebar_child">
                        <a href="#" class="heading_sidebar_anchor_two">
                            Audioable Audiobooks
                            <span class="">
                                <svg class="text-sm " xmlns="http://www.w3.org/2000/svg" width="18" height="18"
                                    viewBox="0 0 24 24">
                                    <path fill="currentColor"
                                        d="M8.025 22L6.25 20.225L14.475 12L6.25 3.775L8.025 2l10 10z" />
                                </svg>
                            </span>
                        </a>
                    </li>



                </ul>

                <div>

                </div>

            </aside>

        </transition>
    </header>
</template>
<script>
import outsideClick from '~/directives/outside-click'
import util from '~/mixin/util'
import { mapGetters, mapActions } from 'vuex'
import SearchPopup from "../../components/SearchPopup";
import Banner from "../../components/Banner";
import Dropdown from "../../components/Dropdown";

export default {
    data() {
        return {
            loading: true,
            headerSticky: false,
            topBannerLoaded: false,
            isTopBannerClosed: true,
            dropdown: false,
            searchPopup: false,
            searchFocused: false,
            searchedText: '',
            sidebarOpen: false,
            showSearchBar: false,
            showCartButton: false,
            showMenuButton: false,
            sidebarItems: [
                { label: 'Home', route: '/' },
                { label: 'About', route: '/about' },
                { label: 'Contact', route: '/contact' }
              
            ]
        }
    },
    computed: {
        isXSmallerDevice() {
            return window.innerWidth <= 576
        },
        headerLeft() {
            return this.headerLinks?.left || []
        },
        headerRight() {
            return this.headerLinks?.right || []
        },
        isPublic() {
            return parseInt(this.topBanner?.status) === this.status.PUBLIC
        },
        isLoggedIn() {
            return this.$auth?.loggedIn || false
        },
        cartCountCom() {
            return this.$auth?.user?.cart_count
        },
        username() {
            return this.$auth?.user?.name?.split(' ')[0]
        },
        email() {
            return this.setting?.email
        },
        phone() {
            return this.setting?.phone
        },
        sellerSignUp() {
            return this.setting?.vendor_registration
        },
        ...mapGetters('language', ['languages', 'currentLanguage']),
        ...mapGetters('common', ['site_setting', 'setting', 'topBanner', 'headerLinks']),
        ...mapGetters('listing', ['searched']),
        ...mapGetters('cart', ['cartCount'])
    },
    watch: {
        cartCountCom(value) {
            this.setCartCount(value)
        },
        '$route'() {
            this.setQFromRoute()
            this.closeDropdown()
        },
        searchedText() {
            if (!this.searchPopup && this.searchFocused) {
                this.emptySearchedSuggestion()
                this.openSearchPopup()
            }
        }
    },
    directives: { outsideClick },
    components: { Dropdown, Banner, SearchPopup },
    mixins: [util],
    methods: {
        handleScroll() {
            if (window.scrollY > 100) {
                this.showSearchBar = true;
                this.showCartButton = true; 
                this.showMenuButton = true; 

            } else {
                this.showMenuButton = false; 

                this.showSearchBar = false;
                this.showCartButton = false; // Hide the cart button when scrolling back up
            }
        },
        handleIntersection(entries) {
            entries.forEach((entry) => {
                this.headerSticky = !entry.isIntersecting
            });
        },
        toggleSidebar() {

            this.sidebarOpen = !this.sidebarOpen;
        },

        async selectedLanguage(data) {
            document.cookie = 'currentLanguage=' + data.key + '; path=/; expires=' + 365 * 60 * 60 * 24
            location.reload()
        },
        topBannerClosed() {
            localStorage.setItem('topBannerClosed', true)
            this.isTopBannerClosed = true
        },
        openSearchPopup() {
            if (this.searchedText.length > 0) {
                this.searchPopup = true
            }
            this.searchFocused = true
        },
        blurSearchInput() {
            this.searchFocused = false
            this.closeSearchPopup()
        },
        closeSearchPopup() {
            setTimeout(() => {
                this.searchPopup = false
            }, 100)
        },
        setQFromRoute() {
            this.searchedText = this.$route?.query?.q || ''
        },
        search() {
            if (this.searchedText && (this.searchedText !== this.searched || this.$route.name !== 'search')) {
                this.$router.push({ path: `/search?q=${this.searchedText}` })
                this.updateSearch(this.searchedText)
            }
        },
        async loggingOut() {
            try {
                await this.$auth.logout()
                this.closeDropdown()
                //this.emptyCartProduct()
            } catch (e) {
                return this.$nuxt.error(e)
            }
        },
        closeDropdown() {
            this.dropdown = false
        },
        ...mapActions('language', ['setDefaultLanguage', 'getLangData']),
        ...mapActions('cart', ['emptyCartProduct', 'setCartCount']),
        ...mapActions('listing', ['updateSearch', 'emptySearchedSuggestion']),
        getLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(this.showLocation, this.showError);
            } else {
                this.city = "Geolocation is not supported by this browser.";
            }
        },
        showLocation(position) {
            const lat = position.coords.latitude;
            const lng = position.coords.longitude;

            fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${lat}&lon=${lng}`)
                .then(response => response.json())
                .then(data => {
                    this.city = data.address.city || data.address.town || data.address.village || data.address.county || 'Unknown';
                    this.picncode = data.address.postcode;
                    this.district = data.address.state_district
                    console.log(data)
                    this.loading = false;
                })
                .catch(error => {
                    console.error('Error fetching city:', error);
                    this.loading = false;
                    this.city = 'Error fetching city';
                });
        },
        showError(error) {
            this.loading = false;
            this.city = `Unable to retrieve location: ${error.message}`;
        }
    },
    deactivated() {
    },
    activated() {

    },
    mounted() {
        
        this.setQFromRoute();
        this.getLocation();
        this.updateSearch(this.searchedText)
        if (this.cartCountCom) {
            this.setCartCount(this.cartCountCom)
        }

        const self = this
        this.$nextTick(() => {
            if (localStorage.getItem('topBannerClosed') !== null) {
                self.isTopBannerClosed = localStorage.getItem('topBannerClosed')
                self.topBannerLoaded = true
            } else {
                self.isTopBannerClosed = false
                self.topBannerLoaded = true
            }
        })


        let rootMargin = '0px 0px 0px 0px'
        if (this.isXSmallerDevice) {
            rootMargin = '40px 0px 0px 0px'
        }


        this.observer = new IntersectionObserver(this.handleIntersection, {
            root: null, 
            rootMargin: rootMargin,
            threshold: 0,
        });
        window.addEventListener('scroll', this.handleScroll);

        // Start observing the target element
        this.observer.observe(this.$refs.headerWrapper);
    },
    
    beforeDestroy() {
        window.removeEventListener('scroll', this.handleScroll);
    }
}
</script>

<style scoped>
.sidebar-home-slide-enter-active,
.sidebar-home-slide-leave-active,
.sidebar-home-fade-enter-active,
.sidebar-home-fade-leave-active {
    transition: all 0.5s ease;
}

.sidebar-home-slide-enter,
.sidebar-home-slide-leave-to,
.sidebar-home-fade-enter,
.sidebar-home-fade-leave-to {
    opacity: .5;
    transform: translateX(-100%);
}

a {
    box-sizing: border-box;
    border: 1px solid transparent;
    border-radius: 4px;
    padding: 4px 0px;
}

.topBar-serach-bar{
    display: flex;
    justify-content: center;
    align-items: center;
    max-width: 100%;
    min-width: 45%;

}
.cart-none{
    display: none;
}
a:hover {

    box-sizing: border-box;
    border: 1px solid #fff;
    border-radius: 4px;

}
.menu-none{
    display: none;
}
.menu-bar{
    margin-right: 10px;
    color: #fff;
    position: relative;
    top: 6px;
}
@media (min-width: 768px) {
    .hide-on-desktop {
        display: none;
    }
}



</style>
