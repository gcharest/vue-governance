<template>
  <div class="container">
    <h1 class="text-center">{{ $t("page.committees.title") }}</h1>
    <div class="accordion mb-3" id="accordion-committees-approach">
      <div class="accordion-item">
        <h2 class="accordion-header" id="approach">
          <button
            class="accordion-button"
            type="button"
            data-bs-toggle="collapse"
            data-bs-target="#collapseOne"
            aria-expanded="true"
            aria-controls="collapseOne"
          >
            {{ $t("page.approach") }}
          </button>
        </h2>
        <div
          id="collapseOne"
          class="accordion-collapse collapse show"
          aria-labelledby="approach"
          data-bs-parent="#accordion-committees-approach"
        >
          <div
            v-html="markdownToHtml($t('page.committees.description'))"
            class="accordion-body"
          ></div>
        </div>
      </div>
    </div>
    <section v-if="!this.$store.getters.isCommitteesEmpty">
      <CommitteePositionMap />
    </section>
    <section v-if="!this.$store.getters.isCommitteesEmpty">
      <CommitteeDetails
        v-for="(committee, elementKey) in committees"
        :key="committee.id"
        :elementKey="elementKey"
        :committee="committee"
        :rates="rates"
        :positions="positions"
      >
      </CommitteeDetails>
    </section>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import CommitteeDetails from "@/components/CommitteeDetails.vue"; // @ is an alias to /src
import CommitteePositionMap from "@/components/CommitteePositionMap.vue";
import { mapState } from "vuex";

@Component({
  computed: { ...mapState(["rates", "positions", "committees", "lang"]) },
  components: {
    CommitteeDetails,
    CommitteePositionMap,
  },
  methods: {
    markdownToHtml(message: string) {
      // eslint-disable-next-line @typescript-eslint/no-var-requires
      const marked = require("marked");
      return marked(message);
    },
  },
})
export default class Home extends Vue {}
</script>
