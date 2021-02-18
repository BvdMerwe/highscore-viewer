<template>
    <div class="container">
		<h2>Fastest ever</h2>
		<h1>Switch it off<br/>and on again-ers</h1>
        <table>
            <thead>
                <tr>
                    <th>Name</th>
                    <th>Time</th>
                    <th>When</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="s in scores" :key="s.player">
                    <td>{{ s.player }}</td>
                    <td>{{ s.value }}</td>
                    <td>{{ s.created }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    data() {
        return {
            scores: [],
        };
    },
    methods: {
        async getData() {
            let scores = [];

            scores = await this.$axios.get(
                "https://highscore.bernardus.co.za/scores?ctx=sioaoa-3310-2021"
            );

            let that = this;
            scores.data.scores.map((s) => {
                s.value = (s.value / 1000).toFixed(2) + " s";
                s.created = that.$moment(s.created).fromNow();
                return s;
            });

			scores.data.scores.sort((a, b) => {
				if (a.value < b.value) {
					return -1;
				} else {
					return 1;
				}
			})

            this.scores = scores.data.scores;
        },
    },
    mounted() {
        this.getData();
    },
};
</script>

<style lang="scss">
body,
html {
    font-family: "04b03", sans-serif;
    background-color: var(--color-light);
    color: var(--color-dark);
	max-width: 500px;
	margin: 0 auto;
	padding-top: 20px;
}

.container {
	h1, h2 {
		text-align: center;
	}
    // position: absolute;
    // top: 50%;
    // left: 50%;
    // transform: translate(-50%, -50%);
    table {
		max-width: 90%;
		margin: 50px auto;
        border-collapse: collapse;
        td,
        th {
            border: 2px solid var(--color-dark);
        }
        th {
            color: var(--color-light);
            background-color: var(--color-dark);
            padding: 6px 12px;
            text-align: center;
            font-weight: normal;
        }
        tbody {
            td {
                padding: 6px 12px;
                text-align: center;
            }
            td:nth-child(2) {
                text-align: right;
            }
        }
    }
}
</style>
