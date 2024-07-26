<template>
  <div>
    <div class="timeline-container">
      <div class="vertical-timeline">
        <div class="timeline-event" v-for="event in events" :key="event.id" :ref="`event-${event.id}`">

          <div class="date" :class="{ 'active-date': currentIndex === event.id - 1 }">{{ event.date }}-</div>
        </div>
      </div>
      <div class="right-container"></div>
    </div>
    <div class="active-event">
      <div class="date"></div>
      <div class="active-event-date">{{ currentEvent.title }}</div>
      <div class="active-event-desc">{{ currentEvent.description }}</div>
    </div>
    <div class="background">
      <img :src="currentEvent.backgroundUrl" alt="The Barrington" />


    </div>
  </div>
</template>

<script>
import anime from 'animejs/lib/anime.es.js';

export default {
  name: 'CompanyTimeline',
  data() {
    return {
      currentIndex: 0,
      events: [
        { id: 1, date: '1989', title: "The Beginning of Ortho Molecular Products", description: 'Ortho Molecular Products is founded with a deep commitment to honoring the practitioner-patient relationship and manufacturing dietary supplements with unsurpassed efficacy.', icon: '/leaf.svg', backgroundUrl: 'https://picsum.photos/1000/1000?random=1' },
        { id: 2, date: '1993', title: "Breakthrough with Natural D-Hist", description: 'Natural D-Hist, the most effective natural product for seasonal support, becomes a breakout product.', icon: '/leaf.svg', backgroundUrl: 'https://picsum.photos/1000/1000?random=2' },
        { id: 3, date: '1998', title: "State-of-the-Art Manufacturing in Wisconsin", description: 'OMP opens a state-of-the-art, cGMP manufacturing facility in Stevens Point, Wisconsin to support rapid growth.', icon: '/leaf.svg' , backgroundUrl: 'https://picsum.photos/1000/1000?random=3' },
        { id: 4, date: '2000', title: "Introducing Orthomega®", description: 'OMP introduces Orthomega®, the first burp-free, high-concentration fish oil for improved results with fewer soft gels.', icon: '/leaf.svg' , backgroundUrl: 'https://picsum.photos/1000/1000?random=4' },
        { id: 5, date: '2003', title: "Major Manufacturing Expansion", description: 'A 60,000-square-foot manufacturing expansion is completed to prepare for long-term growth.', icon: '/leaf.svg', backgroundUrl: 'https://picsum.photos/1000/1000?random=5' },
        { id: 6, date: '2006', title: "Lab Expansion and Ortho Biotic Launch", description: 'Lab expansion provides critical growth for research and testing capabilities. Ortho Biotic is the first clinical strength probiotic to guarantee potency at ingestion without refrigeration.', icon: '/leaf.svg', backgroundUrl: 'https://picsum.photos/1000/1000?random=6' },
        { id: 7, date: '2010', title: "New Corporate Office in Woodstock, Illinois", description: 'OMP opens corporate office in Woodstock, Illinois and expands customer service team to better serve customers.', icon: '/leaf.svg', backgroundUrl: 'https://picsum.photos/1000/1000?random=7' },
        { id: 8, date: '2012', title: "Increased Manufacturing Capacity", description: 'OMP expands again, increasing manufacturing capacity and fulfillment with an 88,000-square-foot facility.', icon: '/leaf.svg', backgroundUrl: 'https://picsum.photos/1000/1000?random=8' },
        { id: 9, date: '2014', title: "Launch of a New User-Friendly Website", description: 'New, more user-friendly website launches.', icon: '/leaf.svg', backgroundUrl: 'https://picsum.photos/1000/1000' },
        { id: 10, date: '2016', title: "International Expansion and Product Redesign", description: 'OMP expands international presence in Canada. OMP announces redesigned product labels, featuring a modern, distinctive look and a unique category-driven system.', icon: '/leaf.svg', backgroundUrl: 'https://picsum.photos/1000/1000?random=9' },
        { id: 11, date: '2017', title: "Second Corporate Office and Strategic Partnerships", description: 'OMP opens second corporate office in Lake Barrington, Illinois. OMP forms exclusive partnerships with HP Ingredients to provide Bergamonte® in Bergamot BPF and Nattopharma to provide MenaQ7® PRO in all vitamin K products.', icon: '/leaf.svg', backgroundUrl: '/thebarrington.jpg' },
        { id: 12, date: '2018', title: "West Coast Distribution Center", description: 'OMP opens West Coast Distribution Center and expands distribution network, providing faster shipping and better service to customers. OMP forms exclusive partnership with Entera Health to provide dairy-free serum-derived bovine immunoglobulins in SBI Protect.', icon: '/leaf.svg', backgroundUrl: 'https://picsum.photos/1000/1000?random=10' },
        { id: 13, date: '2019', title: "Expansion of Manufacturing Facility", description: 'OMP completes expansion of manufacturing facility, nearly doubling square footage.', icon: '/leaf.svg', backgroundUrl: 'https://picsum.photos/1000/1000?random=11' },
        { id: 14, date: '2020', title: "Launch of Turiva", description: 'OMP launches Turiva, a full-spectrum turmeric supplement, which contains an exclusive blend of bioactive phytonutrients that make up the Complete Turmeric Matrix for increased clinical efficacy.', icon: '/leaf.svg', backgroundUrl: 'https://picsum.photos/1000/1000?random=12' },
      ],
    };
  },
  computed: {
    currentEvent() {
      return this.events[this.currentIndex];
    }
  },
  methods: {
    createObserver() {
      const options = {
      root: null,
      rootMargin: '-40% 0px -40% 0px',
      threshold: 0, // Change the threshold value to 0.5 for the halfway point of the viewport
      };

      const callback = (entries, observer) => {
        console.log(observer);
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            console.log(entry.target.id);
            const index = this.events.findIndex(event => `event-${event.id}` === entry.target.id);
            this.currentIndex = index;
          }
        });
      };

      const observer = new IntersectionObserver(callback, options);

      this.events.forEach(event => {
        const target = this.$refs[`event-${event.id}`][0];
        target.id = `event-${event.id}`;
        observer.observe(target);
      });
    },
    animateText() {
      const textContainer = this.$el.querySelector('.active-event');
      anime({
        targets: textContainer,
        opacity: [0, 1],
        translateX: [20, 0],
        duration: 500,
        easing: 'easeInOutQuad',
      });
    }
  },
  watch: {
    currentIndex() {
      this.animateText();
    }
  },
  mounted() {
    this.createObserver();

  }
};
</script>

<style scoped>
.timeline-container {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 600vh;
  padding: 20px;
  margin-top: 300px;
  margin-bottom: 200px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  background-size: cover;
  background-attachment: fixed;
}

.vertical-timeline {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: end;
  justify-content: space-around;
  width: 2px;
  height: 100%;
  margin: 0 auto;
  width: 20%;
  border-right: solid 2px #fff;
}

.right-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 80%;
  height: 100%;
  padding: 10px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.active-event {
  position: fixed;
  width: 600px;
  top: 25vh;
  left: 30vw;
  padding: 20px;
  background: transparent;
}

.timeline-event {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100px;
}

.active-date {
  color: #ffffff;
  font-size: 64px;
  transition: font-size ease-in-out 0.2s;
}

.background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: -1;
  
  
}

.background::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5));
  }

.background img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}


.active-event-date {
  font-size: 48px;
  font-weight: 600;
  text-align: left;
  

}


.active-event-desc {
  font-size: 20px;
  text-align: left;
  margin-top: 20px;
}
</style>