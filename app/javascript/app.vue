<template>
  <div id="app" class="container">
    <h4 class="mt-3 text-center">めもあぷり</h4>
    <div class="row">
      <div class="col-10 offset-1 col-md-6 offset-md-3">
        <div class="form">
          <div class="form-group">
            <label for="title" class="form-label">項目</label>
            <input
              type="text"
              v-model="title"
              placeholder="項目"
              class="form-control"
            />
          </div>
          <div class="form-group">
            <label for="description" class="form-label">詳細</label>
            <input
              type="text"
              v-model="description"
              placeholder="詳細"
              class="form-control"
            />
          </div>
          <button
            class="mt-2 btn btn-secondary"
            @click="addMemo"
            :disabled="!buttonEnabled"
            :title="
              !buttonEnabled ? 'title と descriptionを入力してください' : ''
            "
          >
            メモを追加
          </button>
        </div>
      </div>
    </div>

    <div class="d-flex flex-wrap mt-4">
      <div v-for="memo in memos" :key="memo.id" class="card col-6 col-md-4">
        <div class="card-body">
          <div class="card-title">
            {{ memo.title }}
          </div>
          <div class="card-text">
            {{ memo.description }}<br />
            <button class="btn btn-secondary" v-on:click="deleteMemo(memo.id)">
              削除
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      btnFlg: true,
      memos: "memos",
      title: "",
      description: "",
    };
  },
  computed: {
    buttonEnabled: function () {
      return (this.btnFlg = this.title && this.description ? true : false);
    },
  },
  mounted() {
    this.setMemos();
  },
  methods: {
    setMemos: function () {
      axios.get("/api/memos").then((response) => (this.memos = response.data));
    },
    addMemo: function () {
      axios
        .post("/api/memos", {
          title: this.title,
          description: this.description,
        })
        .then((response) => this.setMemos());
    },
    deleteMemo: function (id) {
      axios
        .delete(`/api/memos/${id}`)
        .then((response) => {
          this.setMemos();
        })
        .catch(function (error) {
          console.log(error);
        });
    },
  },
};
</script>

<style lang="scss" scoped>
#app {
  max-width: 960px;
}
</style>
