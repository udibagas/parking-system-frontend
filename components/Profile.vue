<template>
	<el-dialog
		title="PROFIL SAYA"
		v-loading="loading"
		:visible="show"
		:show-close="false"
	>
		<el-form label-width="180px" label-position="left">
			<el-form-item label="Nama" :class="formErrors.name ? 'is-error' : ''">
				<el-input placeholder="Nama" v-model="formModel.name"></el-input>
				<div class="el-form-item__error" v-if="formErrors.name">
					{{ formErrors.name[0] }}
				</div>
			</el-form-item>

			<el-form-item label="Level">
				<el-input
					disabled
					:value="formModel.role ? 'Admin' : 'Operator'"
				></el-input>
			</el-form-item>

			<el-form-item
				label="Password"
				:class="formErrors.password ? 'is-error' : ''"
			>
				<el-input
					type="password"
					placeholder="Password"
					v-model="formModel.password"
				></el-input>
				<div class="el-form-item__error" v-if="formErrors.password">
					{{ formErrors.password[0] }}
				</div>
			</el-form-item>

			<el-form-item
				label="Konfirmasi Password"
				:class="formErrors.password ? 'is-error' : ''"
			>
				<el-input
					type="password"
					placeholder="Konfirmasi Password"
					v-model="formModel.password_confirmation"
				></el-input>
			</el-form-item>
		</el-form>
		<span slot="footer">
			<el-button @click="$emit('close')" icon="el-icon-error">TUTUP</el-button>
			<el-button type="primary" @click="save" icon="el-icon-success">
				SIMPAN
			</el-button>
		</span>
	</el-dialog>
</template>

<script>
export default {
	props: ['show'],
	data() {
		return {
			formModel: { ...this.$auth.user },
			loading: false,
			formErrors: {},
		}
	},
	methods: {
		save() {
			this.loading = true
			this.$axios
				.$put(`/api/user/${this.formModel.id}`, this.formModel)
				.then((response) => {
					this.$message({
						message: 'Data berhasil diupdate',
						type: 'success',
						showClose: true,
					})
					this.$store.state.user = response
				})
				.catch((e) => {
					if (e.response.status == 422) {
						this.formErrors = e.response.data.errors
					} else {
						this.formErrors = {}
					}
				})
				.finally(() => {
					this.loading = false
				})
		},
	},
}
</script>
