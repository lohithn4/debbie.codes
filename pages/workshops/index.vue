<template>
  <div class="container mt-10">
    <AppBreadCrumb title="Workshops Attended" />
    <AppTitle>Workshops I have attended</AppTitle>
    <AppIntro> A list of workshops that I have attended </AppIntro>
    <div v-for="(workshop, index) in workshops" :key="index" class="flex">
      <WorkshopLinks :workshop="workshop" class="workshop" />
    </div>
  </div>
</template>

<script>
  import gql from 'graphql-tag'
  import { print } from 'graphql/language/printer'
  import gsap from 'gsap'
  import WorkshopLinks from '@/components/WorkshopLinks'
  const QUERY = gql`
    query {
      workshops(order_by: { date: desc }) {
        alt
        blogUrl
        duration
        id
        img
        name
        place
        slidesUrl
        title
        topic
        url
        videoUrl
        year
        date
      }
    }
  `
  export default {
    components: {
      WorkshopLinks
    },
    async asyncData({ app }) {
      const { data } = await app.$hasura({
        query: print(QUERY)
      })
      return {
        workshops: data.workshops
      }
    },
    data() {
      return {
        type: '',
        title: "Debbie's workshops that she has attended",
        description:
          'List of workshops that Debbie has attended around the world'
      }
    },

    head() {
      return {
        title: this.title,
        meta: [
          {
            hid: 'description',
            name: 'description',
            content: this.description
          }
        ],
        link: [
          {
            hid: 'canonical',
            rel: 'canonical',
            href: `https://debbie.codes/workshops`
          }
        ]
      }
    },
    mounted() {
      gsap.fromTo(
        '.workshop',
        { opacity: 0 },
        {
          opacity: 1,
          duration: 0.5,
          ease: 'power1.out',
          stagger: {
            each: 0.1,
            from: 'bottom'
          }
        }
      )
    }
  }
</script>
