<template>
  <div>
    <button
      type="button"
      class="btn btn-primary bg-info border-info my-2"
      data-toggle="modal"
      data-target="#branchModal"
    >
      On branch: <b>{{ currentBranch }}</b>
    </button>

    <div
      class="modal fade"
      id="branchModal"
      tabindex="-1"
      aria-labelledby="branchModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="branchModalLabel">Branches</h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>

          <div class="modal-body">
            <div class="list-group">
              <button
                class="list-group-item list-group-item-action bg-info border-info active"
                data-dismiss="modal"
                :key="0"
              >
                <b>{{ currentBranch }}</b>
              </button>
              <button
                class="list-group-item list-group-item-action"
                data-dismiss="modal"
                v-for="(branch, i) in branches"
                :key="i + 1"
                @click="changeCurrentBranch(branch)"
              >
                {{ branch }}
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['currentBranch', 'branches', 'changeCurrentBranch'],
};
</script>
