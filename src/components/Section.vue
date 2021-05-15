<template>
  <div style="padding: 0 20px">
    <div class="section__head">
      <h3 v-if="!section.updating" @click="section.updating = true">
        {{ section.title }}
      </h3>
      <input
        v-else
        type="text"
        v-model="section.title"
        @keydown.enter="$emit('updateSectionTitle', index, section.title)"
      />

      <div class="section__head__actions">
        <img
          @click.prevent="$emit('addNewTodo', index)"
          src="@/assets/images/plus.png"
        />
        <img
          @click.prevent="$emit('deleteSection', index)"
          src="@/assets/images/trash.png"
        />
      </div>
    </div>

    <div
      class="section"
      :style="{
        background: section.todos.length < 1 ? '#EBEFF1' : 'transparent',
      }"
    >
      <draggable
        v-model="section.todos"
        group="todos"
        :emptyInsertThreshold="150"
      >
        <transition-group type="animation">
          <Todo
            v-for="(todo, todoIndex) in section.todos"
            @showTodo="$emit('showTodo', $event)"
            @deleteTodo="$emit('deleteTodo', $event)"
            :sectionIndex="index"
            :key="todo + todoIndex"
            :todo="todo"
          />
        </transition-group>
      </draggable>

      <div
        v-if="section.todos.length < 1"
        class="section__add_task"
        @click="$emit('addNewTodo', index)"
      >
        <img src="@/assets/images/plus.png" />
        Add Task
      </div>
    </div>
  </div>
</template>

<script>
import Todo from "@/components/Todo";
import draggable from "vuedraggable";

export default {
  components: {
    Todo,
    draggable,
  },
  props: {
    index: {
      type: Number,
      required: true,
    },
    section: {
      type: Object,
      required: true,
      default: () => {},
    },
  },
};
</script>

<style lang="scss" scoped>
.section {
  width: 250px;
  height: 800px;
  overflow-y: auto;
  border-radius: 10px;
  transition: all 0.2s ease-in;
  cursor: move;
  padding: 10px;

  &::-webkit-scrollbar {
    width: 2px;
  }

  /* Track */
  &::-webkit-scrollbar-track {
    background: #f1f1f1;
  }

  /* Handle */
  &::-webkit-scrollbar-thumb {
    background: #ddd;
  }

  &__head {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: #f9f9f9;
    padding: 0 0 20px 0px;

    input {
      width: 150px;
      border-radius: 5px;
      outline: none;
      border: 1px solid #ddd;
    }

    h3 {
      font-weight: 700;
    }

    &__actions {
      display: flex;
      align-items: center;
      gap: 10px;

      img {
        width: 12px;
        height: 12px;
        cursor: pointer;
      }
    }
  }

  &__add_task {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 10px;
    margin: 10px;
    user-select: none;
    cursor: pointer;

    img {
      width: 16px;
      height: 16px;
      margin-right: 10px;
    }
  }

  &__add_task:hover {
    background: #dfe4e5;
  }
}

// .section:active {
//   //box-shadow:  0px 10px 12px -15px rgba(0, 0, 0, 0.56);
//   background: #ddd !important
// }
</style>
