<template>
  <el-menu class="header-menu  shepherd-project" :unique-opened="true" mode="horizontal" default-active="1" router>
    <!-- 不激活项目路由-->
    <el-menu-item index="1" v-show="false">Placeholder</el-menu-item>
    <el-submenu index="2" popper-class="submenu">
      <template v-slot:title>
        <el-tooltip effect="light" placement="right" :enterable="false">
          <div slot="content">{{ currentProject }}</div>
          <span class="project-name">
            {{ $t('commons.project') }}: {{ currentProject }}
          </span>
        </el-tooltip>
      </template>
      <project-search-list :current-project.sync="currentProject"/>
      <div v-permission="['WORKSPACE_PROJECT_MANAGER:READ']">
        <el-divider/>
      </div>
      <el-menu-item :index="'/setting/project/create'" v-permission="['WORKSPACE_PROJECT_MANAGER:READ+CREATE']">
        <font-awesome-icon :icon="['fa', 'plus']"/>
        <span style="padding-left: 7px;">{{ $t("project.create") }}</span>
      </el-menu-item>
      <el-menu-item :index="'/setting/project/:type'" v-permission="['WORKSPACE_PROJECT_MANAGER:READ']">
        <font-awesome-icon :icon="['fa', 'list-ul']"/>
        <span style="padding-left: 7px;">{{ $t('commons.show_all') }}</span>
      </el-menu-item>
    </el-submenu>
  </el-menu>
</template>

<script>
import ProjectSearchList from "./ProjectSearchList";
import {PROJECT_NAME} from "../../utils/constants";
import {getCurrentProjectID} from "../../utils/token";


export default {
  name: "ProjectSwitch",
  props: {
    projectName: String
  },
  components: {ProjectSearchList},
  watch: {
    currentProject() {
      sessionStorage.setItem(PROJECT_NAME, this.currentProject);
    }
  },
  methods: {
    getCurrentProjectID,
  },
  data() {
    return {
      currentProject: this.projectName
    };
  }
};
</script>

<style scoped>
.project-name {
  display: inline-block;
  width: 160px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.el-divider--horizontal {
  margin: 0;
}
</style>
