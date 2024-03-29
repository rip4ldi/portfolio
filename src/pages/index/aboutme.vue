<template>
  <div>
    <h1 class="title">{{ translation.title }}</h1>
    <div class="skills">
      <div v-for="skill in skills" :key="skill.id" class="skill">
        <h2 class="subtitle">{{ translation.subtitles[skill.id] }}</h2>
        <div class="cards">
          <a v-for="item in skill[skill.id]" :key="item.id" :href="item.link" target="_blank" class="card">
            <div class="banner" :style="{ backgroundImage: `url('${item.image}')` }"></div>
            <h3 class="skill-name">{{ item.name }}</h3>
            <div class="skill-level">
              <span v-if="['languages', 'design'].includes(skill.id)" class="percentage">{{ item.level }}%</span>
              <span v-if="['os'].includes(skill.id)" class="percentage">{{ `${item.years} ${item.years < 2 ? translation.year : translation.years }` }}</span>
              <div class="skill-level-bar">
                <div class="background"></div>
                <div v-if="['languages', 'design'].includes(skill.id)" class="bar" :style="{ backgroundColor: item.color, width: `${item.level}%` }"></div>
                <div v-if="['os'].includes(skill.id)" class="bar" :style="{ backgroundColor: item.color, width: `${item.years * 100 / total_years}%` }"></div>
              </div>
            </div>
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useStore } from "vuex";
import en from "../../assets/translation/en.json";
import fr from "../../assets/translation/fr.json";

const translations = { en, fr };

const store = useStore();
store.commit("change_page", "aboutme");

const translation = ref(translations[store.state.language].pages.index.aboutme);

const total_years = ref(3);
const skills = ref([
  {
    id: "languages",
    languages: [
      {
        name: "HTML",
        level: 90,
        id: "html5",
        color: "#e14d25",
        image: require("../../assets/images/skills/html5.png"),
        link: "https://developer.mozilla.org/fr/docs/Web/HTML"
      },
      {
        name: "CSS",
        level: 90,
        id: "css3",
        color: "#006eba",
        image: require("../../assets/images/skills/css3.png"),
        link: "https://developer.mozilla.org/fr/docs/Web/CSS"
      },
      {
        name: "JavaScript",
        level: 90,
        id: "javascript",
        color: "#f0db4f",
        image: require("../../assets/images/skills/javascript.png"),
        link: "https://developer.mozilla.org/fr/docs/Web/JavaScript"
      },
      {
        name: "Vue.js",
        level: 80,
        id: "vuejs",
        color: "#42b883",
        image: require("../../assets/images/skills/vuejs.png"),
        link: "https://vuejs.org"
      },
      {
        name: "Nuxt.js",
        level: 70,
        id: "nuxtjs",
        color: "#00dc82",
        image: require("../../assets/images/skills/nuxtjs.png"),
        link: "https://nuxtjs.org"
      },
      {
        name: "Sass",
        level: 70,
        id: "sass",
        color: "#bf4080",
        image: require("../../assets/images/skills/sass.png"),
        link: "https://sass-lang.com"
      },
      {
        name: "Adonis.js",
        level: 80,
        id: "adonisjs",
        color: "#5a45ff",
        image: require("../../assets/images/skills/adonisjs.png"),
        link: "https://adonisjs.com"
      },
      {
        name: "ExpressJS",
        level: 90,
        id: "expressjs",
        color: "#aeaeae",
        image: require("../../assets/images/skills/expressjs.png"),
        link: "http://expressjs.com"
      },
      {
        name: "PostgreSQL",
        level: 70,
        id: "postgres",
        color: "#306091",
        image: require("../../assets/images/skills/postgresql.png"),
        link: "https://www.postgresql.org"
      },
      {
        name: "MongoDB",
        level: 100,
        id: "mongodb",
        color: "#46b04e",
        image: require("../../assets/images/skills/mongodb.png"),
        link: "https://www.mongodb.com"
      },
      {
        name: "C++",
        level: 5,
        id: "cplusplus",
        color: "#06407f",
        image: require("../../assets/images/skills/cplusplus.png"),
        link: "https://docs.microsoft.com/cpp/cpp"
      },
      {
        name: "Git",
        level: 70,
        id: "git",
        color: "#ee5132",
        image: require("../../assets/images/skills/git.png"),
        link: "https://git-scm.com"
      },
    ],
  },
  {
    id: "os",
    os: [
      {
        name: "Windows",
        years: 3,
        id: "windows",
        color: "#00abed",
        image: require("../../assets/images/skills/windows.png"),
        link: "https://www.microsoft.com/windows"
      },
      {
        name: "Kali",
        years: 1.5,
        id: "kali",
        color: "#2785f3",
        image: require("../../assets/images/skills/kali.png"),
        link: "https://www.kali.org"
      },
      {
        name: "Ubuntu",
        years: 1,
        id: "ubuntu",
        color: "#f47320",
        image: require("../../assets/images/skills/ubuntu.png"),
        link: "https://ubuntu.com"
      }
    ],
  },
  {
    id: "design",
    design: [
      {
        name: "Blender",
        level: 30,
        id: "blender",
        color: "#fd6e1e",
        image: require("../../assets/images/skills/blender.png"),
        link: "https://www.blender.org"
      },
      {
        name: "Figma",
        level: 80,
        id: "figma",
        color: "#66bb6a",
        image: require("../../assets/images/skills/figma.png"),
        link: "https://www.figma.com"
      },
      {
        name: "Adobe Illustrator",
        level: 50,
        id: "illustrator",
        color: "#ff5722",
        image: require("../../assets/images/skills/illustrator.png"),
        link: "https://www.adobe.com/products/illustrator.html"
      },
      {
        name: "Adobe Premiere Pro",
        level: 70,
        id: "premiere-pro",
        color: "#e040fb",
        image: require("../../assets/images/skills/premiere-pro.png"),
        link: "https://www.adobe.com/products/premiere.html"
      }
    ]
  }
]);
</script>

<style lang="scss" scoped>
.content {
  padding: 7rem 10%;

  .title, .subtitle {
    color: var(--text-color);
  }
  
  .skills {
    margin: 1rem 0;

    .skill {
      .cards {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr;
        grid-gap: 50px 32px;
        gap: 50px 32px;
        margin: 1.5rem 0 2rem;

        .card {
          background-color: var(--card-color);

          .banner {
            background-repeat: no-repeat;
            background-position: center;
            background-size: 100% 180px;
            border-radius: 6px 6px 0 0;
            height: 170px;
            width: 100%;
            transition: 0.2s;
          }

          &:hover { 
            .banner { background-size: 110% 200px; }
          }

          .skill-name {
            color: var(--text-color);
            padding: 0.5rem .75rem 0;
          }

          .skill-level {
            display: flex;
            align-items: center;
            justify-content: flex-start;
            padding: 0.5rem 0.75rem;
            width: 100%;

            .percentage {
              color: var(--text-color);
              white-space: nowrap;
            }

            .skill-level-bar {
              position: relative;
              margin: 0 20px;
              width: 100%;

              .bar {
                position: absolute;
                border-radius: 6px;
                height: 5px;
              }

              .background {
                position: absolute;
                background-color: #fff;
                border-radius: 6px;
                height: 5px;
                width: 100%;
              }
            }
          }
        }
      }
    }
  }
}

@media only screen and (max-width: 1500px) {
  .content {
    .skills {
      .skill {
        .cards {
          grid-template-columns: 1fr 1fr;
        }
      }
    }
  }
}

@media only screen and (max-width: 1000px) {
  .content {
    .skills {
      .skill {
        .cards {
          grid-template-columns: 1fr;
        }
      }
    }
  }
}

@media only screen and (max-width: 600px) {
  .content {
    .skills {
      .skill {
        .cards {
          .card {
            .banner {
              background-size: 100% 150px;
              height: 140px;
            }
          }
        }
      }
    }
  }
}
</style>