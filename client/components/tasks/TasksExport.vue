<template>
  <div class="tasks-export">
    <v-dialog
      :value="active"
      max-width="420"
      @input="$emit('update:active')"
    >
      <v-card>
        <v-card-title class="headline">
          {{ $t("Export") }}
        </v-card-title>
        <v-divider />
        <v-card-text>
          <v-progress-linear v-if="loading" indeterminate absolute top />
          <v-list>
            <v-list-item @click="exportODS()">
              <v-list-item-avatar>
                <v-icon class="blue white--text">
                  mdi-google-spreadsheet
                </v-icon>
              </v-list-item-avatar>

              <v-list-item-content>
                <v-list-item-title>
                  ODS
                </v-list-item-title>
                <v-list-item-subtitle>
                  OpenDocument Format - LibreOffice Calc
                </v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>

            <v-list-item @click="exportXLSX()">
              <v-list-item-avatar>
                <v-icon class="green white--text">
                  mdi-google-spreadsheet
                </v-icon>
              </v-list-item-avatar>

              <v-list-item-content>
                <v-list-item-title>
                  XLSX
                </v-list-item-title>
                <v-list-item-subtitle>
                  Excel
                </v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card-text>
        <v-divider />
        <v-card-actions>
          <v-spacer />
          <v-btn v-shortkey="['esc']" text @click="close()" @shortkey="close()">
            {{ this.$t("Close") }}
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import { Meteor } from "meteor/meteor";

export default {
  props: {
    projectId: {
      type: String,
      default: ""
    },
    active: Boolean
  },
  data() {
    return {
      loading: false
    };
  },
  methods: {
    close() {
      this.$emit("update:active", false);
    },

    exportODS() {
      if (this.loading) return;
      this.loading = true;
      Meteor.call("tasks.exportProject", { projectId: this.projectId, format: "ods" }, (error, result) => {
        this.loading = false;
        if (error) {
          this.$notifyError(error);
          return;
        }
        const blob = new Blob([result.data], { type: "application/vnd.oasis.opendocument.spreadsheet" });
        saveAs(blob, "task.ods");
      });
    },

    exportXLSX() {
      if (this.loading) return;
      this.loading = true;
      Meteor.call("tasks.exportProject", { projectId: this.projectId, format: "xlsx" }, (error, result) => {
        this.loading = false;
        if (error) {
          this.$notifyError(error);
          return;
        }
        const blob = new Blob([result.data], { type: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet" });
        saveAs(blob, "task.xlsx");
      });
    }
  }
};
</script>

<style scoped>
</style>
