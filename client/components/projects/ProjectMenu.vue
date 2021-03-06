<template>
  <div class="project-menu">
    <project-history ref="projectHistory" :project-id="projectId" />
    <project-trashcan ref="projectTrashcan" :project-id="projectId" />

    <v-list v-if="projectId" class="pt-0">
      <v-list-item :to="{ name: 'project-dashboard', params: { projectId: projectId } }">
        <v-list-item-action>
          <v-icon>mdi-information-outline</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>{{ $t("Dashboard") }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item :to="{ name: 'project', params: { projectId: projectId } }">
        <v-list-item-action>
          <v-icon>mdi-format-list-bulleted</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>{{ $t("Tasks") }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item
        :to="{ name: 'project-bpmn', params: { projectId: projectId } }"
      >
        <v-list-item-action>
          <v-icon>mdi-chart-donut</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>{{ $t("BPMN") }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item
        :to="{ name: 'project-timeline', params: { projectId: projectId } }"
      >
        <v-list-item-action>
          <v-icon>mdi-chart-timeline-variant</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>{{ $t("Planning") }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item
        :to="{ name: 'project-canvas', params: { projectId: projectId } }"
      >
        <v-list-item-action>
          <v-icon>mdi-file-document-box-check</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>{{ $t("Canvas") }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item
        :to="{ name: 'project-weather', params: { projectId: projectId } }"
      >
        <v-list-item-action>
          <v-icon>mdi-white-balance-sunny</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>{{ $t("Weather") }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item
        :to="{
          name: 'project-attachments-page',
          params: { projectId: projectId }
        }"
      >
        <v-list-item-action>
          <v-icon>mdi-attachment</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>{{ $t("Attachments") }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item @click="$refs.projectHistory.open()">
        <v-list-item-action>
          <v-icon>mdi-history</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>{{ $t("History") }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item @click="$refs.projectTrashcan.open()">
        <v-list-item-action>
          <v-icon>mdi-delete</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>{{ $t("Trashcan") }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item
        v-if="canManageProject(projectId)"
        :to="{ name: 'project-settings', params: { projectId: projectId } }"
      >
        <v-list-item-action>
          <v-icon>mdi-settings</v-icon>
        </v-list-item-action>
        <v-list-item-content>
          <v-list-item-title>{{ $t("Settings") }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list>
    <v-divider />
    <labels :project-id="projectId" />
  </div>
</template>

<script>
import { Permissions } from "/imports/api/permissions/permissions";

export default {
  props: {
    projectId: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      showHistoryDialog: false
    };
  },
  methods: {
    canManageProject(projectId) {
      return (
        Permissions.isAdmin(Meteor.userId(), projectId)
        || Permissions.isAdmin(Meteor.userId())
      );
    }
  }
};
</script>
