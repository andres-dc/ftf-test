<template>
  <div id="app" class="container mt-4">
    <div
      class="d-flex flex-column flex-md-row justify-content-between align-items-start align-items-md-center mb-2"
    >
      <Title :url="url" :owner="owner" :repository="name" />
      <BranchList
        :currentBranch="currentBranch"
        :branches="branches"
        :changeCurrentBranch="changeCurrentBranch"
      />
    </div>
    <CommitHistory :commits="commits" />
    <Footer />
  </div>
</template>

<script>
import Title from './components/Title';
import CommitHistory from './components/CommitHistory';
import BranchList from './components/BranchList';
import Footer from './components/Footer';
import { getFromGithub } from './utils/apiCalls';

const owner = 'andres-dc';
const repository = 'ftf-test';

export default {
  name: 'App',
  components: {
    Title,
    BranchList,
    CommitHistory,
    Footer,
  },
  data() {
    return {
      owner: owner,
      name: repository,
      currentBranch: null,
      branches: [],
      commits: [],
      url: `https://github.com/${owner}/${repository}`,
    };
  },
  methods: {
    async changeCurrentBranch(branch) {
      this.currentBranch = branch;
      await this.fetchAllData();
    },
    async getDefaultBranch() {
      await getFromGithub('repos', owner, repository).then((res) => {
        this.currentBranch = res.data.default_branch;
      });
    },
    async getCommits() {
      this.commits = [];
      await getFromGithub(
        'repos',
        owner,
        repository,
        `commits?sha=${this.currentBranch}`
      ).then((res) => {
        res.data.forEach((el) => {
          const commit = {
            commitURL: el.html_url,
            message: el.commit.message,
            user: el.author.login,
            userURL: el.author.html_url,
            userAvatarURL: el.author.avatar_url,
            date: el.commit.author.date,
            sha: el.sha,
          };

          this.commits.push(commit);
        });
      });
    },
    async getBranches() {
      this.branches = [];
      await getFromGithub('repos', owner, repository, 'branches').then(
        (res) => {
          res.data.forEach((el) => {
            if (el.name !== this.currentBranch) {
              this.branches.push(el.name);
            }
          });
        }
      );
    },
    async fetchAllData() {
      await this.getCommits();
      await this.getBranches();
    },
  },
  async created() {
    await this.getDefaultBranch();
    await this.fetchAllData();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 0.9rem;
}

a {
  text-decoration: none;
  color: black;
  font-weight: bold;
}
</style>
