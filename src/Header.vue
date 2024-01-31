<template>
    <header>
        <div class="header-container">
            <div class="logo">
                <img src="https://gumam.com/wp-content/uploads/2022/09/Group-4.png" alt="Gumam">
            </div>
            <div class="navigation-container">
                <i class="fa fa-bars" id="navIcon" @click="toggleMobileNavigation()"></i>
                <nav>
                    <i class="fa fa-times" id="closeIcon" @click="closeMobileNavigation()" style="font-size: 24px; width: 24px; height: 24px; color: #FFFFFF;"></i>
                    <ul class="d-flex justify-content-between align-items-center list-unstyled m-0">
                        <li v-for="(item, index) in header" :key="index">
                            <span class="dropbtn" @click="allowClick($event)">
                                <a :class="item.title" :href="item.url">{{ item.title }}</a>
                                <i v-if="item.children.length > 0" class="fa fa-chevron-down" style="font-size: 14px; margin-left: 5px;"></i>
                            </span>
                            <div class="dropdown-content" v-if="item.children.length > 0">
                                <a v-for="(child, index) in item.children"  :href="child.url" :key="index">{{ child.title }}</a>
                            </div>
                        </li>
                    </ul>
                </nav>
            </div>
        </div>
    </header>

</template>

<script>
    import axios from 'axios';

    export default {
        components: { },
        // eslint-disable-next-line vue/multi-word-component-names
        name: 'Header',
        created() {
            if ("HEADER_FOOTER_SETTINGS" in window) {
                this.fetchNavigation(window.HEADER_FOOTER_SETTINGS.apiUri);
            } else {
                if (this.url) {
                    this.fetchNavigation(this.url);
                }
            }

              // Set the initial value for windowWidth
            this.windowWidth = window.innerWidth;

            // Add an event listener to track window width changes
            window.addEventListener('resize', this.handleResize);

        },
        data() {
            return {
                logo: require("@/assets/images/gumam-logo.png"),
                header: {},
                windowWidth: 0
            }
        },

        computed: {
        },

        watch: {
            '$data.windowWidth'() {
                const navigation = document.querySelector('.navigation-container nav');

                if (this.windowWidth > 1120) {
                navigation.classList.remove('active');
                }
            }
        },

        methods: {
            fetchNavigation(apiUri) {
                axios.get(apiUri)
                    .then((response) => {
                        this.header = response.data.header.schema;
                    })
            },

            handleResize() {
                this.windowWidth = window.innerWidth;
            },

            toggleMobileNavigation() {
                const nav = document.querySelector(".navigation-container nav");
                nav.classList.toggle("active");
            },

            closeMobileNavigation() {
                const nav = document.querySelector(".navigation-container nav");
                
                // Check if the 'active' class is present
                if (nav.classList.contains("active")) {
                    // 'active' class is present, remove it
                    nav.classList.remove("active");
                }
            },

            allowClick(event) {

                console.log("Element: ", event.target)

                const parentLi = event.target.closest('li');

                if (!parentLi) {
                    return; // No <li> parent found, do nothing
                }

                const nav = parentLi.querySelector('.dropdown-content');

                if(!nav) {
                    return;
                }

                if (window.innerWidth < 1120) {
                    event.preventDefault();
                }

                console.log(nav)

                // Check if the 'active' class is present
                if (nav.classList.contains("active")) {
                    // 'active' class is present, remove it
                    nav.classList.remove("active");
                } else {
                    // 'active' class is not present, add it
                    nav.classList.add("active");
                }
                event.stopPropagation(); // Prevent the click event from propagating to the document
            },

        }
    }
</script>

<style scoped>
    @import './assets/css/main.css';
</style>
