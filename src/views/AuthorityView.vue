<template>
  <div>
    <el-tree
      ref="treeRef"
      :data="list"
      show-checkbox
      node-key="roleId"
      check-strictly="true"
      :default-checked-keys="authority"
      :props="{
        children: 'roleList',
        label: 'name',
      }"
    />
    <el-button @click="changeAuthority">确认修改</el-button>
  </div>
</template>

<script lang="ts">
import { InitData } from "../type/authority";
import { defineComponent, onMounted, reactive, toRefs } from "vue";
import { useRoute } from "vue-router";
import { getAuthorityList } from "../request/api";
export default defineComponent({
  setup() {
    const route = useRoute();
    console.log(route);
    const params: any = route.params;
    const data = reactive(new InitData(params.id, params.authority));

    onMounted(() => {
      getAuthorityList().then((res) => {
        console.log(res);
        data.list = res.data;
      });
    });
    const changeAuthority = () => {
      console.log(
        data.treeRef.getCheckedKeys().sort(function (a: number, b: number) {
          return a - b;
        })
      );
    };
    return { ...toRefs(data), changeAuthority };
  },
  components: {},
});
</script>

<style scoped></style>
