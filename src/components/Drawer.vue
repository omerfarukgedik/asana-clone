<template>
  <div class="drawer">
    {{ todo.title }}
    <span>{{ todo.section }}</span>

    <hr />

    <ul>
      <li v-for="key in Object.keys(todo)" :key="key">
        <div style="width: 150px; text-transform: uppercase; font-weight: 500">
          {{ key }}
        </div>

        <div style="text-transform: capitalize">
          {{ todo[key] }}
        </div>
      </li>
    </ul>

    <div style="margin-top: 40px">
      Sub todos ({{ todo.sub_todos.length }})
      <hr />

      <div class="sub_todo" v-for="subTodo in 5" :key="subTodo">
        <img src="@/assets/images/trash.png" />
        <span
          >Lorem Ipsum is simply dummy text of the printing and typesetting
          industry. Lorem Ipsum has been the industry's standard dummy text ever
          since the 1500s
        </span>
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
  },
};
</script>

<style lang="scss" scoped>
.drawer {
  position: absolute;
  top: 0;
  right: 0;
  width: 600px;
  overflow: hidden;
  height: 100vh;
  padding-left: 0;
  border-left: 2px solid #ddd;
  background: #f9f9f9;
  z-index: 200;
  transition: all 0.2s;
  padding: 20px;
  font-size: 1rem;
  font-weight: 700;
  display: flex;
  flex-direction: column;
  color: #233e8b;
  overflow-y: scroll;

  span {
    font-weight: 400;
    margin-top: 10px;
  }

  hr {
    margin: 20px 0;
  }

  ul {
    list-style-type: none;
    margin: 0;
    padding: 0;

    li {
      display: flex;
      align-items: center;
      margin-top: 10px;
    }
  }

  .sub_todo {
    display: flex;
    align-items: center;
    border-bottom: 1px solid #ddd;
    padding: 10px;

    img {
      width: 16px;
      height: 16px;
      margin-right: 20px;
    }
  }
}
</style>
