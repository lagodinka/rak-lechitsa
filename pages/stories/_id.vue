<template>
  <container>
    <profile :source="getProfile" />
    <fullStory :source="getProfile" />
    <div class="cards-container">
      <card v-for="obj in getPieceOfStories" :key="obj.id" :source="obj" />
    </div>
    <nuxt-link to="/stories" class="link_underline_false">
      <banner theme="light"><span>Больше статей</span></banner>
    </nuxt-link>
  </container>
</template>

<script>
import Card from '@/components/Card';
import WidthAdjustContainer from '@/components/WidthAdjustContainer';
import Banner from '@/components/Banner';
import StoryProfile from '@/components/StoryProfile';
import FullStory from '@/components/FullStory';

export default {
  components: {
    container: WidthAdjustContainer,
    card: Card,
    banner: Banner,
    profile: StoryProfile,
    fullStory: FullStory,
  },
  computed: {
    profile() {
      return this.$store.getters['stories/getStories'];
    },
    stories() {
      return this.$store.getters['stories/getStories'];
    },
    getId() {
      const indx = this.$route.path.search(/\/\d{1,}/);
      return this.$route.path.slice(indx + 1);
    },
    getProfile() {
      let ind = this.getId;
      return this.stories.find(item => item.id == ind);
    },
    getPieceOfStories() {
      if (process.browser) {
        let copy = this.stories.slice(0);
        let stories = [];
        if (window.innerWidth <= 768) {
          this.storiesOnPage = 3;
        }
        if (window.innerWidth <= 425) {
          this.storiesOnPage = 2;
        }
        stories = copy.splice(0, this.storiesOnPage);
        return stories;
      }
    },
  },
  data() {
    return {
      storiesOnPage: 4,
    };
  },
};
</script>

<style scoped>
.cards-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-column-gap: 40px;
}

.link_underline_false {
  text-decoration: none;
  color: inherit;
}

@media screen and (max-width: 1024px) {
  .cards-container {
    grid-column-gap: 30px;
  }
}
@media screen and (max-width: 768px) {
  .cards-container {
    grid-template-columns: 1fr 1fr 1fr;
    grid-column-gap: 20px;
  }
}
@media screen and (max-width: 400px) {
  .cards-container {
    grid-template-columns: 1fr;
    grid-template-rows: 1fr 1fr;
    grid-row-gap: 30px;
  }
}
</style>
