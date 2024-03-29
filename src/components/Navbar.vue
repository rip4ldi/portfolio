<template>
  <div>
    <div v-if="sidebar_enable" @click="sidebar_close()" class="calque"></div>
    <header class="navbar">
      <div class="sidebar-button" @click="sidebar_toggle">
        <font-awesome-icon icon="bars" type="fa" class="icon" />
      </div>
      <router-link to="/" class="navbar-title">squarfiuz</router-link>
      <div class="flex-grow"></div>
      <nav class="nav-links">
        <div v-for="navlink in navlinks" :key="navlink.id" class="item">
          <router-link v-if="navlink.type === 'router-link'" :to="navlink.link" :class="`link ${store.state.page === navlink.id ? 'active' : ''}`">
            {{ translation.navlinks[navlink.id] }}
          </router-link>
          <a v-if="navlink.type === 'link'" :href="navlink.link" target="_blank" class="link">
            {{ translation.navlinks[navlink.id] }}
            <font-awesome-icon type="fa" icon="arrow-up-right-from-square" class="icon" />
          </a>
          <div v-if="navlink.type === 'dropdown-link'" class="dropdown-link">
            <div class="dropdown">
              {{ translation.navlinks[navlink.id] }}
              <font-awesome-icon type="fa" icon="caret-down" class="icon" />
            </div>
            <ul class="options">
              <li v-for="option in navlink.options" :key="option.id" class="option">
                <div v-if="option.type === 'button'" class="item">
                  <font-awesome-icon :style="`opacity: ${store.state[navlink.id] === option.id ? '1' : '0'}`" type="fa" icon="caret-right" class="icon" />
                  <div v-if="navlink.id === 'language'" :class="`button ${store.state.language === option.id ? 'active' : ''}`" @click="change_language(option.id)">{{ option.name }}</div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </nav>
      <div class="change-theme" @click="change_theme" :aria-label="`Switch to ${store.state.is_dark ? 'light' : 'dark'} theme`">
        <font-awesome-icon v-if="store.state.is_dark" icon="moon" type="fa" />
        <font-awesome-icon v-else icon="sun" type="fa" />
      </div>
    </header>
    <aside :class="`sidebar ${sidebar_enable ? 'open' : ''}`">
      <nav class="nav-links">
        <div v-for="navlink in navlinks" :key="navlink.id" class="item">
          <router-link v-if="navlink.type === 'router-link'" :to="navlink.link" :class="`link ${store.state.page === navlink.id ? 'active' : ''}`" @click="sidebar_close()">
            {{ translation.navlinks[navlink.id] }}
          </router-link>
          <a v-if="navlink.type === 'link'" :href="navlink.link" target="_blank" class="link">
            {{ translation.navlinks[navlink.id] }}
            <font-awesome-icon type="fa" icon="arrow-up-right-from-square" class="icon" />
          </a>
          <div v-if="navlink.type === 'dropdown-link'" :class="`dropdown-link ${dropdown_active.includes(navlink.id) ? 'active' : ''}`">
            <div class="dropdown" @click="dropdown_toggle(navlink.id)">
              {{ translation.navlinks[navlink.id] }}
              <font-awesome-icon type="fa" icon="caret-down" class="icon" />
            </div>
            <ul class="options">
              <li v-for="option in navlink.options" :key="option.id" class="option">
                <div v-if="option.type === 'button'" class="item">
                  <font-awesome-icon :style="`opacity: ${store.state[navlink.id] === option.id ? '1' : '0'}`" type="fa" icon="caret-right" class="icon" />
                  <div v-if="navlink.id === 'language'" :class="`button ${store.state.language === option.id ? 'active' : ''}`" @click="change_language(option.id)">{{ option.name }}</div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </nav>
    </aside>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router"
import { useStore } from "vuex";
import en from "../assets/translation/en.json";
import fr from "../assets/translation/fr.json";

const translations = { en, fr };
const router = useRouter();

const store = useStore();

const translation = translations[store.state.language].components.Navbar;

const sidebar_enable = ref(false);

const dropdown_active = ref([]);

const navlinks = [
  {
    id: "home",
    link: "/",
    type: "router-link"
  },
  {
    id: "aboutme",
    link: "/aboutme",
    type: "router-link"
  },
  {
    id: "contactme",
    link: "/contactme",
    type: "router-link"
  },
  {
    id: "language",
    type: "dropdown-link",
    options: [
      {
        name: "English",
        id: "en",
        type: "button"
      },
      {
        name: "Français",
        id: "fr",
        type: "button"
      }
    ]
  },
  {
    name: "GitHub",
    id: "github",
    link: "https://github.com/squarfiuz/portfolio",
    type: "link"
  }
];

function change_language(language) {
  store.commit("change_language", language);
  localStorage.language = language;
  router.go();
}

function change_theme() {
  store.commit("change_theme");
}

function sidebar_toggle() {
  sidebar_enable.value = !sidebar_enable.value;
};

function sidebar_close() {
  sidebar_enable.value = false;
};

function dropdown_toggle(name) {
  if (dropdown_active.value.includes(name)) {
    dropdown_active.value = dropdown_active.value.filter(item => item !== name);
  } else {
    dropdown_active.value.push(name);
  }
}


</script>

<style lang="scss" scoped>
.calque {
  z-index: var(--z-index-calque);
  position: fixed;
  display: none;
  background-color: #000;
  opacity: 0.6;
  height: 100%;
  width: 100%;
}

.nav-links {
  display: flex;
  align-items: center;

  .link {
    display: flex;
    align-items: center;
    background: linear-gradient(to top, var(--link-color-over) 0%, var(--link-color-over) 10%, transparent 10.01%) no-repeat left bottom / 0 100%;
    color: var(--link-color);
    margin-left: 1.5rem;
    font-size: 1.2rem;
    font-weight: 700;
    transition: background-size 0.3s;

    &:hover, &.active { background-size: 100% 100%; }

    .icon {
      fill: #a3a3a3;
      margin-left: 10px;
      height: 20px;
      width: 20px;
    }
  }
}

.dropdown-link {
  position: relative;

  .dropdown {
    cursor: pointer;
    display: flex;
    align-items: center;
    color: var(--link-color);
    margin-left: 1.5rem;
    font-size: 1.2rem;
    font-weight: 700;

    .icon {
      fill: #a3a3a3;
      margin-left: 10px;
      height: 18px;
      width: 18px;
    }
  }

  .options {
    display: none;
    top: 24px;
    right: -8px;
    background-color: var(--dropdown-color);
    color: var(--text-color);
    list-style: none;
    border-radius: 6px;
    padding: 12px 0;
    min-width: 128px;
    box-shadow: 0 12px 32px rgba(0, 0, 0, 0.1), 0 2px 6px rgba(0, 0, 0, 0.08);

    .item {
      display: flex;
      align-items: center;

      .button {
        cursor: pointer;
        background: linear-gradient(to top, var(--link-color-over) 0%, var(--link-color-over) 10%, transparent 10.01%) no-repeat left bottom / 0 100%;
        margin-left: 8px;
        line-height: 32px;
        transition: background-size 0.3s;

        &:hover, &.active { background-size: 100% 100%; }
      }

      .icon {
        fill: #a3a3a3;
        margin-left: 10px;
        height: 18px;
        width: 18px;
      }
    }
  }
}

.navbar {
  position: fixed;
  top: 0;
  right: 0;
  left: 0;
  z-index: var(--z-index-navbar);
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: var(--navbar-color);
  padding: 0 2.5rem;
  height: var(--header-height);
  width: 100%;

  .sidebar-button {
    cursor: pointer;
    display: none;
    align-items: center;
    justify-content: center;
    padding: 0.75rem;

    .icon {
      fill: var(--sidebar-icon-color);
      height: 1.5rem;
      width: 1.5rem;
    }
  }

  .flex-grow { flex-grow: 1; }

  .navbar-title {
    color: var(--text-color);
    font-size: 1.3rem;
    font-weight: 600;
  }

  .nav-links {
    .dropdown-link {
      position: relative;

      &:hover {
        .options { display: block; }
      }

      .options {
        position: absolute;
        top: 26px;
        right: -8px;
      }
    }
  }
}

.sidebar {
  display: none;
  position: fixed;
  top: var(--header-height);
  bottom: 0;
  left: 0;
  z-index: var(--z-index-sidebar);
  overflow-y: auto;
  background-color: var(--navbar-color);
  width: 16.4rem;
  transform: translateX(-100%);
  transition: transform 0.25s ease;

  &.open { transform: translateX(0); }

  .nav-links {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    padding: 1rem 1.5rem;

    .link { line-height: 46px; }

    .dropdown-link {
      line-height: 46px;

      .options {
        background-color: transparent;
        box-shadow: none;
        padding: 5px 18px 0;
      }

      &.active {
        .options { display: block; }
      }
    }
  }
}

.change-theme {
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: var(--theme-button-color);
  padding: 0.5rem;
  margin-left: 1.5rem;
  border: none;
  border-radius: 25%;

  &:hover { background-color: var(--theme-button-color-hover); }

  svg {
    fill: var(--theme-icon-color);
    height: 1.5rem;
    width: 1.5rem;
  }
}

@media only screen and (max-width: 900px) {
  .calque { display: block; }

  .navbar {
    .sidebar-button { display: flex; }

    .nav-links { display: none; }
  }

  .sidebar { display: block; }
}
</style>