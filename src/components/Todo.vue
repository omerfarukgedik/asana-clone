<template>
  <div class="todo">
    <div class="todo__head">
      <div
        class="todo__head__priority"
        @click.self="$emit('showTodo', { todo, sectionIndex })"
        :style="{ background: getColor(todo.priority) }"
      />
      <img
        @click="$emit('deleteTodo', { todoId: todo.id, sectionIndex })"
        src="@/assets/images/trash.png"
      />
    </div>

    <div class="todo__content">
      {{ todo.title }}
    </div>

    <div class="todo__footer">
      {{ todo.dueDate }}
      <div class="todo__footer__subtask">
        {{ todo.sub_todos.length }}
        <img src="@/assets/images/list.png" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    todo: {
      type: Object,
      required: true,
    },
    sectionIndex: {
      type: Number,
      required: true,
    },
  },
  methods: {
    getColor(priority) {
      if (priority === "high") return "#40d651";
      if (priority === "medium") return "#bc93c1";
      if (priority === "low") return "#2e97ad";
      return "#000";
    },
  },
};
</script>

<style lang="scss" scoped>
.todo {
  padding: 10px;
  background: white;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-bottom: 10px;
  box-shadow: 0px 10px 12px -15px rgba(0, 0, 0, 0.56);
  cursor: grab;
  user-select: none;

  &__head {
    display: flex;
    align-items: center;
    justify-content: space-between;

    &__priority {
      width: 80px;
      height: 8px;
      border-radius: 10px;
      background: coral;
      cursor: pointer;
    }
    img {
      cursor: pointer;
      width: 14px;
      height: 14px;
      padding: 2px;
    }
    img:hover {
      background: #ddd;
      border-radius: 5px;
    }
  }

  &__content {
    font-weight: 700;
    padding: 10px 0;
  }

  &__footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 0.8rem;

    &__subtask {
      display: flex;
      align-items: center;

      img {
        width: 16px;
        height: 16px;
        margin-left: 8px;
      }
    }
  }
}

.todo:hover {
  background: #f9f9f9;
  transition: all 0.2s;
}
</style>
