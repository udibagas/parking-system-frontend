<template>
	<el-row :gutter="20" class="mb-3">
		<el-col :span="8">
			<el-card>
				Hari Ini
				<div class="text-5xl text-purple-700">
					Rp. {{ $decimal(summary.today) }}
				</div>
			</el-card>
		</el-col>
		<el-col :span="8">
			<el-card>
				Bulan Ini
				<div class="text-5xl text-blue-700">
					Rp. {{ $decimal(summary.this_month) }}
				</div>
			</el-card>
		</el-col>
		<el-col :span="8">
			<el-card>
				Total
				<div class="text-5xl text-orange-700">
					Rp. {{ $decimal(summary.total) }}
				</div>
			</el-card>
		</el-col>
	</el-row>
</template>

<script>
export default {
	data() {
		return {
			summary: {},
		}
	},

	methods: {
		getData() {
			this.$axios.$get('/api/summary').then((r) => (this.summary = r))
		},
	},

	created() {
		this.getData()
	},
}
</script>
