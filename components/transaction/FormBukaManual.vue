<template>
	<el-dialog
		title="FORM BUKA MANUAL"
		center
		:visible.sync="show"
		:before-close="closeForm"
	>
		<el-form label-position="left" label-width="200px">
			<el-form-item
				label="Alasan buka manual"
				:class="{ 'is-error': formErrors.alasan }"
			>
				<el-input
					autofocus
					type="textarea"
					v-model="formModel.alasan"
					rows="3"
					placeholder="Alasan buka manual"
				></el-input>
				<div class="el-form-item__error" v-if="formErrors.alasan">
					{{ formErrors.alasan[0] }}
				</div>
			</el-form-item>

			<el-form-item
				v-if="this.gateOutList.length > 1"
				label="Gate Keluar"
				:class="{ 'is-error': formErrors.gate_out_id }"
			>
				<el-select
					v-model="formModel.gate_out_id"
					style="width: 100%"
					placeholder="Gate Keluar"
				>
					<el-option
						v-for="gate in gateOutList"
						:key="gate.id"
						:label="gate.nama"
						:value="gate.id"
					></el-option>
				</el-select>
				<div class="el-form-item__error" v-if="formErrors.gate_out_id">
					{{ formErrors.gate_out_id[0] }}
				</div>
			</el-form-item>

			<el-form-item
				label="Masukkan password Admin"
				:class="formErrors.password ? 'is-error' : ''"
			>
				<el-input
					type="password"
					v-model="formModel.password"
					placeholder="Password"
				></el-input>
				<div class="el-form-item__error" v-if="formErrors.password">
					{{ formErrors.password[0] }}
				</div>
			</el-form-item>
		</el-form>

		<div slot="footer">
			<el-button icon="el-icon-error" @click="closeForm"> BATAL </el-button>
			<el-button icon="el-icon-success" type="primary" @click="save">
				SIMPAN
			</el-button>
		</div>
	</el-dialog>
</template>

<script>
export default {
	props: ['show', 'gateOutList'],

	data() {
		return {
			formModel: {},
			formErrors: {},
		}
	},

	mounted() {
		if (this.gateOutList.length == 1) {
			this.formModel.gate_out_id = this.gateOutList[0].id
		}
	},

	methods: {
		closeForm() {
			this.formModel = {}
			this.formErrors = {}
			this.$emit('close')
		},

		save() {
			this.$confirm(
				'Aksi ini akan dicatat oleh sistem. Anda yakin?',
				'Peringatan',
				{ type: 'warning' }
			)
				.then(() => {
					this.$axios
						.$post('/api/manualOpenLog', this.formModel)
						.then((r) => {
							this.$message({
								message: r.message,
								type: 'success',
							})
							this.$emit('open-gate', this.formModel.gate_out_id)
							this.closeForm()
						})
						.catch((e) => {
							if (e.response.status == 422) {
								this.formErrors = e.response.data.errors
							}
						})
				})
				.catch((e) => console.log(e))
		},
	},
}
</script>
