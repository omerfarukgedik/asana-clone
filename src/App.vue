<template>
  <div>
    <div class="wrapper" @mousewheel="wheel" ref="wrapper">
      <draggable v-if="sections.length > 0" v-model="sections">
        <transition-group class="wrapper__section_wrapper" type="animation">
          <Section
            v-for="(section, index) in sections"
            :key="section + index"
            :index="index"
            :section="section"
            @deleteSection="deleteSection"
            @updateSectionTitle="updateSectionTitle"
            @addNewTodo="addNewTodo"
            @deleteTodo="deleteTodo"
            @showTodo="showTodo"
          />
        </transition-group>
      </draggable>

      <div>
        <div class="add_section_btn" @click.prevent="addNewSection">
          <img src="@/assets/images/plus.png" />
          Add Section
        </div>
      </div>
    </div>

    <Drawer v-if="isDrawerMenuOpen" :todo="selectedTodo" />

    <div
      class="drawer-mask"
      :style="{
        width: isDrawerMenuOpen ? '100vw' : '0',
        opacity: isDrawerMenuOpen ? '0.4' : '0',
      }"
      @click="
        isDrawerOpen = false;
        selectedTodo = {};
      "
    ></div>
  </div>
</template>

<script>
import Section from "@/components/Section";
import Drawer from "@/components/Drawer";
import draggable from "vuedraggable";

export default {
  name: "App",
  components: {
    Section,
    Drawer,
    draggable,
  },
  data() {
    return {
      sections: [],
      todos: [],
      scrool: 0,
      isDrawerOpen: true,
      selectedTodo: {},
    };
  },
  computed: {
    isDrawerMenuOpen: function () {
      return Object.keys(this.selectedTodo).length > 0 && this.isDrawerOpen;
    },
  },
  methods: {
    generateId(array) {
      return array[array.length - 1]?.id + 1 || 1;
    },
    addNewSection() {
      let id = this.generateId(this.sections);
      this.sections.push({
        id,
        title: `New Section ${id}`,
        updating: false,
        todos: [],
      });
    },
    addNewTodo(sectionIndex) {
      let id = this.generateId(this.sections[sectionIndex].todos);
      this.sections[sectionIndex].todos.push({
        id,
        priority: "high",
        dueDate: "24 Şub",
        title: `New Task ${id}`,
        description: "New Task Description",
        sub_todos: [],
      });
    },
    deleteSection(index) {
      this.sections.splice(index, 1);
    },
    deleteTodo({ todoId, sectionIndex }) {
      let index = this.sections[sectionIndex].todos.findIndex(
        (t) => t.id === todoId
      );
      this.sections[sectionIndex].todos.splice(index, 1);
    },
    updateSectionTitle(index, title) {
      this.sections[index].title = title;
      this.sections[index].updating = false;
    },
    showTodo({ todo, sectionIndex }) {
      this.selectedTodo = todo;
      this.selectedTodo.section = this.sections[sectionIndex].title;
      this.isDrawerOpen = true;
    },
    scrollTo(element, scrollPixels, duration) {
      const scrollPos = element.scrollLeft;
      // Condition to check if scrolling is required
      if (
        !(
          (scrollPos === 0 || scrollPixels > 0) &&
          (element.clientWidth + scrollPos === element.scrollWidth ||
            scrollPixels < 0)
        )
      ) {
        // Get the start timestamp
        const startTime =
          "now" in window.performance
            ? performance.now()
            : new Date().getTime();

        const scroll = (timestamp) => {
          //Calculate the timeelapsed
          const timeElapsed = timestamp - startTime;
          //Calculate progress
          const progress = Math.min(timeElapsed / duration, 1);
          //Set the scrolleft
          element.scrollLeft = scrollPos + scrollPixels * progress;
          //Check if elapsed time is less then duration then call the requestAnimation, otherwise exit
          if (timeElapsed < duration) {
            //Request for animation
            window.requestAnimationFrame(scroll);
          } else {
            return;
          }
        };
        //Call requestAnimationFrame on scroll function first time
        window.requestAnimationFrame(scroll);
      }
    },
    wheel(e) {
      if (e.deltaY < 0) this.scrollTo(this.$refs.wrapper, -400, 300);
      else this.scrollTo(this.$refs.wrapper, 400, 300);
    },
  },
  async created() {
    const response = await fetch("/data.json");
    const data = await response.json();
    this.sections = data.sections;
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@200&display=swap");

$bg-color: #f9f9f9;
$font: "Montserrat", sans-serif;

$priority_high: #40d651;
$priority_low: #2e97ad;
$priority_medium: #bc93c1;

html::-webkit-scrollbar {
  display: none;
}

.drawer-mask {
  position: absolute;
  left: 0;
  top: 0;
  height: 100vh;
  background: #000;
  opacity: 0.3;
  z-index: 199;
  transition: opacity 0.2s;
}

body {
  margin: 0;
  padding: 0;
  * {
    margin: 0;
    font-size: 0.9rem;
  }

  background-color: #{$bg-color};
  font-family: $font;

  .wrapper {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #484c50;
    display: flex;
    gap: 1rem;
    padding: 2rem 1rem;
    overflow-x: auto;

    &::-webkit-scrollbar {
      width: 1px;
    }

    /* Track */
    &::-webkit-scrollbar-track {
      background: #f1f1f1;
    }

    /* Handle */
    &::-webkit-scrollbar-thumb {
      background: #ddd;
      border-radius: 20px;
    }

    &__section_wrapper {
      display: flex;
    }

    .add_section_btn {
      display: flex;
      align-items: center;
      user-select: none;
      min-width: 130px;

      img {
        width: 12px;
        height: 12px;
        margin-right: 10px;
      }

      &:hover {
        color: #4ca1a3;
        cursor: pointer;
        font-weight: 700;
      }
    }
  }
}
</style>
