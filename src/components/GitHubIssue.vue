<template>
	<div class="container">
		<div v-show="loader.getIssue">
			<img src="/static/loading.svg" alt="Carregando">
		</div>
		<div v-show="!loader.getIssue && issue.number">
			<h1>Issue #{{ issue.number }}</h1>
			 <h2>{{ issue.title }}</h2>
			<div>{{ issue.body }}</div>
			<br>
			<a class="btn btn-warning" href="javascript:history.go(-1)">Voltar</a>
			</div>
	</div>
</template>

<script>
import axios from 'axios';

export default{
	name: 'GitHubIssue',
	created() {
		this.getIssue();
	},
	data() {
	  return {
			issue: {},
			loader: {
				getIssue: false,
			},
		};
	},
	methods: {
		getIssue() {
			this.loader.getIssue = true;
			const url = `https://api.github.com/repos/${this.$route.params.name}/${this.$route.params.repo}/issues/${this.$route.params.issue}`;
			axios.get(url).then((response) => {
				this.issue = response.data;
			}).catch((error) => {
				// eslint-disable-next-line
				console.log(error);
			}).finally(() => {
				this.loader.getIssue = false;
			});
		},
	},
};

</script>
