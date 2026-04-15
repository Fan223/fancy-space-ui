<template>
  <div>
    <FancyTree
      :data="categories"
      label-key="name"
      :expanded-keys="expandedKeys"
      :selected-key="selectedKey"
      @toggle="toggle"
      @select="select"
    >
      <template #node="{ node }">
        <span> {{ node.orderNum }} {{ node.flag }} </span>
      </template>
    </FancyTree>
  </div>
</template>

<script lang="ts" setup>
const expandedKeys = ref<string[]>([]);

const toggle = (node: Node) => {
  const i = expandedKeys.value.indexOf(node.id);
  if (i > -1) expandedKeys.value.splice(i, 1);
  else expandedKeys.value.push(node.id);
};

const selectedKey = ref<string>();

const select = (node: Node) => {
  selectedKey.value = node.id;

  if (node.type === "article") {
    console.log("跳转:", node.path);
  }
};

const categories = [
  {
    id: "50807724088414208",
    parentId: null,
    name: "开发环境",
    orderNum: 1,
    flag: "Y",
    count: "8",
    children: [],
  },
  {
    id: "3346001472081920",
    parentId: null,
    name: "开发工具",
    orderNum: 10,
    flag: "Y",
    count: "12",
    children: [],
  },
  {
    id: "13879500616560640",
    parentId: null,
    name: "开发技术及框架",
    orderNum: 20,
    flag: "Y",
    count: "15",
    children: [],
  },
  {
    id: "22187186852577280",
    parentId: null,
    name: "开发知识及其他",
    orderNum: 30,
    flag: "Y",
    count: "14",
    children: [],
  },
  {
    id: "13879831178047488",
    parentId: null,
    name: "Java",
    orderNum: 40,
    flag: "Y",
    count: "4",
    children: [
      {
        id: "68556025489289216",
        parentId: "13879831178047488",
        name: "Java 集合",
        orderNum: 41,
        flag: "N",
        count: null,
        children: [
          {
            id: "68556025489289216",
            parentId: "13879831178047488",
            name: "Java 集合",
            orderNum: 41,
            flag: "N",
            count: null,
            children: [],
          },
        ],
      },
      {
        id: "68503586945921024",
        parentId: "13879831178047488",
        name: "Java 并发",
        orderNum: 41,
        flag: "N",
        count: "5",
        children: [],
      },
    ],
  },
  {
    id: "13879548351934464",
    parentId: null,
    name: "Spring",
    orderNum: 50,
    flag: "Y",
    count: "11",
    children: [],
  },
  {
    id: "13879653255671808",
    parentId: null,
    name: "服务器",
    orderNum: 60,
    flag: "Y",
    count: "7",
    children: [],
  },
  {
    id: "22194500523376640",
    parentId: null,
    name: "操作系统及使用",
    orderNum: 70,
    flag: "Y",
    count: "7",
    children: [],
  },
  {
    id: "22196140840173568",
    parentId: null,
    name: "数据结构与算法",
    orderNum: 80,
    flag: "Y",
    count: "7",
    children: [],
  },
  {
    id: "15565191134134272",
    parentId: null,
    name: "计算机网络",
    orderNum: 90,
    flag: "Y",
    count: "2",
    children: [],
  },
  {
    id: "15571596780060672",
    parentId: null,
    name: "数据库",
    orderNum: 100,
    flag: "Y",
    count: "7",
    children: [],
  },
  {
    id: "14212701838987264",
    parentId: null,
    name: "面试",
    orderNum: 110,
    flag: "Y",
    count: "1",
    children: [],
  },
  {
    id: "50840364587909120",
    parentId: null,
    name: "Python",
    orderNum: 470,
    flag: "Y",
    count: "2",
    children: [],
  },
  {
    id: "13879727343857664",
    parentId: null,
    name: "Vue",
    orderNum: 480,
    flag: "Y",
    count: "6",
    children: [],
  },
  {
    id: "24343994444177408",
    parentId: null,
    name: "Flutter",
    orderNum: 490,
    flag: "Y",
    count: "2",
    children: [],
  },
  {
    id: "13879595646906368",
    parentId: null,
    name: "设计模式",
    orderNum: 500,
    flag: "Y",
    count: "8",
    children: [],
  },
  {
    id: "14151607879553024",
    parentId: null,
    name: "未发布",
    orderNum: 999,
    flag: "N",
    count: null,
    children: [],
  },
];
</script>

<style lang="scss" scoped></style>
