<template>
  <footer>
      <div class="footer-container">
          <div class="footer-top">
              <div class="footer-top-container">
                  <nav>
                      <ul>
                          <li v-for="(location, index) in locations" @click="showContent(index)" :key="'location_item_' + index" :class="['large', {'active' : activeIndex === index}]">
                                  <h5>{{ location.title }}</h5>
                                  <span><i class="fa fa-chevron-down" style="color: #ffa700;"></i></span>
                          </li>

                          <div v-for="(location, index) in locations" :key="'location_item_mobile' + index" class="location_mobile_block">
                              <li  @click="showContentMobile(index)" :class="{'active' : activeIndex === index}">
                                  <h5>{{ location.title }}</h5>
                                  <span><i class="fa fa-chevron-down" style="color: #ffa700;"></i></span>
                              </li>
                              <div :class="['footer-nav-content', {'active' : activeIndex === index}]">
                                  <div>
                                      <div>
                                          <h3>Kontakt info</h3>
                                      </div>
                                      <div class="footer-nav-location-info">
                                          <div>
                                              <span v-for="(left, index) in location.children[0].children" :key="'footer-nav-location-info-left_' + index">
                                                  <p v-if="!left.meta.mail && !left.meta.tel">{{ left.title }}</p>
                                                  <a v-if="left.meta.tel" :href="'tel:+' + left.meta.tel">{{ left.title }}</a>
                                                  <a v-if="left.meta.mail" :href="'mailto:' + left.meta.mail">{{ left.title }}</a>
                                              </span>
                                          </div>
                                          <div>
                                              <span v-for="(right, index) in location.children[1].children" :key="'footer-nav-location-info-left_' + index">
                                                  <p v-if="!right.meta.mail && !right.meta.tel">{{ right.title }}</p>
                                                  <a v-if="right.meta.tel" :href="'tel:+' + right.meta.tel">{{ right.title }}</a>
                                                  <a v-if="right.meta.mail" :href="'mailto:' + right.meta.mail">{{ right.title }}</a>
                                              </span>
                                          </div>
                                      </div>
                                  </div>
                                  <div>
                                      <img :src="location.icon" :alt="location.title">
                                  </div>
                              </div>
                          </div>
                      
                      </ul>
                  </nav>
                  <div class="footer-nav-content-container">
                      <div v-for="(location, index) in locations" :class="['footer-nav-content', {'active' : activeIndex === index}]" :id="index + 1" :key="'footer-nav-content_' + index">
                          <div>
                              <div>
                                  <h3>Kontakt info</h3>
                              </div>
                              <div class="footer-nav-location-info">
                                  <div>
                                      <span v-for="(left, index) in location.children[0].children" :key="'footer-nav-location-info-left_' + index">
                                          <p v-if="!left.meta.mail && !left.meta.tel">{{ left.title }}</p>
                                          <a v-if="left.meta.tel" :href="'tel:+' + left.meta.tel">{{ left.title }}</a>
                                          <a v-if="left.meta.mail" :href="'mailto:' + left.meta.mail">{{ left.title }}</a>
                                      </span>
                                  </div>
                                  <div>
                                      <span v-for="(right, index) in location.children[1].children" :key="'footer-nav-location-info-left_' + index">
                                          <p v-if="!right.meta.mail && !right.meta.tel">{{ right.title }}</p>
                                          <a v-if="right.meta.tel" :href="'tel:+' + right.meta.tel">{{ right.title }}</a>
                                          <a v-if="right.meta.mail" :href="'mailto:' + right.meta.mail">{{ right.title }}</a>
                                      </span>
                                  </div>
                              </div>
                          </div>
                          <div>
                              <img :src="location.icon" :alt="location.title">
                          </div>
                      </div>
                  </div>
              </div>
          </div>
          <div class="footer-bottom">
              <div class="footer-info">
                  <div>
                      <a :href="url">
                          <img src="https://gumam.com/wp-content/uploads/2022/09/Group-4.png" alt="">
                      </a>
                  </div>
                  <div class="footer-icon-group footer-icon-group-mobile">
                      <span v-for="(social, index) in social_media" :key="index">
                          <a :href="social.url" :target="social.target"><img :src="social.icon" :alt="social.title"></a>
                      </span>
                  </div>
                  <div class="footer-contact">
                      <span>
                          <i class="fa fa-phone" style="color: #ffa700; font-size: 38px;"></i>
                      </span>
                      <span>
                          <p>{{ meta.contact_title }}</p>
                          <a v-if="meta.contact_number !== undefined" :href="'tel:+' + removeSpaces(meta.contact_number)">{{ meta.contact_number }}</a>
                      </span>
                  </div>
              </div>
              <div class="footer-links">
                  <div class="footer-working-hours">
                      <p>{{ meta.week }} <strong>{{ meta.week_working_hours }}</strong></p>
                      <p>{{ meta.saturday }} <strong>{{ meta.saturday_working_hours }}</strong></p>
                      <p>{{ meta.sunday }} <strong>{{ meta.sunday_working_hours }}</strong></p>
                      <div class="footer-icon-group">
                          <span v-for="(social, index) in social_media" :key="index">
                              <a :href="social.url" :target="social.target"><img :src="social.icon" :alt="social.title"></a>
                          </span>
                      </div>
                  </div>
                  <div class="footer-details">
                      <div v-for="(footerItem, index) in footer" :key="'footer_item_' + index">
                          <span class="menu-container" @click="toggleList(transformToId(footerItem.title))">
                              <h5>{{ footerItem.title }}</h5>
                              <span class="icon" :id="transformToId(footerItem.title) + '_icon'"><i class="fa fa-chevron-down" style="color: #ffa700;"></i></span>
                          </span>
                          <ul :id="transformToId(footerItem.title)" class="dropdown">
                              <li v-for="(footerLink, index) in footerItem.children" :key="'footer_item_' + index">
                                  <a :href="footerLink.url" :target="footerLink.title">
                                      {{ footerLink.title }}
                                  </a>
                              </li>
                          </ul>
                      </div>
                  </div>
              </div>
              <div class="footer-rights" v-for="(right, index) in rights" :key="index">
                  <a :href="right.url" :target="right.target">{{ right.title }}</a>
              </div>
          </div>
      </div>
  </footer>
</template>

<script>
  import axios from "axios";

  export default {
    // eslint-disable-next-line vue/multi-word-component-names
      name: "Footer",

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

          if (this.windowWidth < 943) {
              this.activeIndex = null;
          }
      },

      data() {
          return {
              footer: [],
              rights: [],
              social_media: [],
              meta: [],
              locations: [],
              url: '',
              activeIndex: 0,
              windowWidth: 0,
              isFirstTime: true,
          }
      },
      computed: {

      },

      methods: {
          fetchNavigation(apiUri) {
              axios.get(apiUri)
                  .then((response) => {
                      this.footer = response.data.footer.schema;
                      this.social_media = response.data.social_media.schema;
                      this.rights = response.data.rights.schema;
                      this.meta = response.data.meta
                      this.locations = response.data.locations.schema
                      this.url = response.data.site_url
                  })
          },

          transformToId(name) {
              let newName = name.toLowerCase(); // Use toLowerCase instead of toLowerCase

              // Replace spaces with underscores globally using a regular expression
              newName = newName.replace(/ /g, '_');

              return newName;
          },

          showContent(index) {
              this.activeIndex = index;
          },

          showContentMobile(index) {
              if (this.activeIndex === index) {
                  this.activeIndex = null;
              } else {
                  this.activeIndex = index;
              }
          },

          removeSpaces(number) {
            return number.replace(/\s/g, '');
          },

          handleResize() {
              this.windowWidth = window.innerWidth;

              if (this.windowWidth < 943) {
                  if (this.isFirstTime) {
                      this.activeIndex = null;
                      this.isFirstTime = false;
                  }

              } else if (this.windowWidth >= 943 && this.activeIndex === null) {
                  this.activeIndex = 0;
                  this.isFirstTime = true;
              }
          },

          toggleList(listId) {
              if (this.windowWidth < 943) {
                  var ul = document.getElementById(listId);
                  var icon = document.getElementById(listId + "_icon");
                  ul.classList.toggle('active');
                  icon.classList.toggle('active');
              }
          }
      }
  }
</script>

<style scoped>
</style>
