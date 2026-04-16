<template>
  <div mt-6>
    <FancyTree
      label="name"
      :data="categories"
      :expanded="expanded"
      :selected="selected"
      @toggle="toggle"
      @select="select"
    >
      <template #node="{ node }">
        <b>{{ node.name }} </b>
      </template>
    </FancyTree>
  </div>
</template>

<script lang="ts" setup>
interface Category {
  id: string;
  parentId: string | null;
  name: string;
  orderNum: number;
  flag: string;
  count: string | null;
  children: Category[];
}

const expanded = ref<string[]>([]);

const toggle = (node: Category) => {
  console.log(node);

  const i = expanded.value.indexOf(node.id);
  if (i > -1) expanded.value.splice(i, 1);
  else expanded.value.push(node.id);
};

const selected = ref<string>();

const select = (node: Category) => {
  selected.value = node.id;
};

const categories: Category[] = [
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
    name: "开发知识及其他开发知识及其他开发知识及其他",
    orderNum: 30,
    flag: "Y",
    count: "14",
    children: [],
  },
  {
    id: "13879831178047488",
    parentId: null,
    name: "Java 开发知识及其他开发知识及其他开发知识及其他",
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
        count: "5",
        children: [
          {
            id: "68556025489289217",
            parentId: "13879831178047488",
            name: "List",
            orderNum: 41,
            flag: "N",
            count: "3",
            children: [],
          },
          {
            id: "68556025489289218",
            parentId: "13879831178047488",
            name: "Set",
            orderNum: 41,
            flag: "N",
            count: "1",
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
];
</script>
