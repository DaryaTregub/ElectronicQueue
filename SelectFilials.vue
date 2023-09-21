<style>
.scroll_employeers {
	height: 31vw;
}

.scroll_turn {
	height: 22vw;
}

.scroll_turn_none {
	height: 19.3vw;
}

.turn_container {
	padding: 0;
}

.btn-change_container {
	margin-top: 3vw;
}
</style>
<template>
	<!--begin::Main-->
	<!--begin::Root-->
	<div class="d-flex flex-column flex-root">
		<!--begin::Page-->
		<div class="page d-flex flex-row flex-column-fluid">
			<!--begin::Wrapper-->
			<div class="wrapper d-flex flex-column flex-row-fluid" id="kt_wrapper">

				<!--begin::Content wrapper-->
				<div class="d-flex flex-column-fluid">

					<!--begin::Container-->
					<div class="d-flex flex-column flex-column-fluid container-fluid">
						<!--begin::Post-->
						<div class="content flex-column-fluid" id="kt_content">
							<div class="row g-5 g-xl-10 mb-5 mb-xl-10">
								<!--begin::Card-->
								<div class="card">
									<!--begin::Card body-->
									<div class="card-body">

										<!--begin::Heading-->
										<div class="text-center">

											<div class="col-lg-12 fv-row">
												<select name="filials" id="filials" data-control="select2"
													class="form-select form-select-solid form-select-lg fw-bold">
													<FilialsItemVue></FilialsItemVue>
												</select>
											</div>

										</div>
										<!--end::Heading-->
									</div>
									<!--end::Card body-->
								</div>
								<!--end::Card-->
							</div>
							<div class="row g-5 g-xl-10 mb-5 mb-xl-10">
								<div class="turn_container">
									<!--begin::Card-->
									<div class="card" v-show="showCard">
										<!--begin::Card body-->
										<div class="card-body d-flex" id="EmpsCard">
											<div class="d-flex flex-column col-3">
												<div class="scroll px-5 scroll_employeers">
													<ul
														class="nav nav-tabs nav-pills border-0 flex-row flex-md-column me-5 mb-3 mb-md-0 fs-6">
														<EmployeersItemVue :emps="emps" @click-emp="empsClick"
															:activeEmp="activeEmp">
														</EmployeersItemVue>
													</ul>
												</div>
											</div>
											<div class="dragg col-9">
												<TurnDraggVue v-if="empSelected" :turns="empSelected.queues">
												</TurnDraggVue>
												<div class="btn-change_container" v-if="empSelected">
													<span
														class="btn btn-outline btn-outline-dashed btn-outline-warning btn-active-light-warning w-100"
														id="change_btn" @click="changeClick">Отправить заявку</span>
												</div>
											</div>

										</div>
										<!--end::Card body-->
									</div>
									<!--end::Card-->
								</div>

							</div>
						</div>
						<!--end::Post-->
					</div>
					<!--end::Container-->
				</div>
				<!--end::Content wrapper-->
			</div>
			<!--end::Wrapper-->
		</div>
		<!--end::Page-->
	</div>

	<!--end::Root-->
	<!--end::Main-->

	<!--begin::Javascript-->
</template>
<script>
import FilialsItemVue from './FilialsItem.vue';
import EmployeersItemVue from './EmployeersItem.vue';
import TurnDraggVue from './TurnDragg.vue';
import axios from 'axios';
export default {
	components: {
		FilialsItemVue,
		EmployeersItemVue,
		TurnDraggVue,
	},

	data() {
		return {
			respond: null,
			emps: null,
			showCard: false,
			showTurn: false,
			empSelected: null,
			changeData: { EmpRid: null, filialUuid: null, Queues: null },
			activeEmp: null
		}
	},

	mounted() {
		this.selectActive();
		this.getFilials();
	},

	methods: {
		selectActive() {
			$(document).ready(function () { $("#filials").select2(); });
		},

		getFilials() {
			$('#filials').on('select2:select', (e) => {
				this.data = e.params.data;
				axios
					.get(`api/v1_0/Orient?type=getEmps&filialUuid=` + this.data.id)
					.then((respond, status, jqXHR) => {
						if (typeof respond.error === 'undefined') {
							this.respond = respond.data;
							this.emps = JSON.parse(JSON.stringify(this.respond)).emps;
							this.showCard = true;
							this.$emit('select-change', this.emps);
							console.log(this.emps)
						};
					})
					.catch(function (error) {
						console.log(error);
					});
			});
		},
		empsClick(value) {
			this.empSelected = null;
			this.showTurn = true;
			this.empSelected = value;
			console.log(this.empSelected.rid)
			this.activeEmp = this.empSelected.rid;
		},

		changeClick() {
			this.changeData.EmpRid = this.empSelected.rid
			this.changeData.filialUuid = this.data.id
			this.changeData.Queues = this.empSelected.queues
			axios
				.post('api/v1_0/Orient?type=CreateNewTicket', this.changeData)
				.then(function (response) {
					if (response.data.type == "ok") {
						console.log(response)
						Swal.fire({
							text: "Заявка отправлена",
							icon: "success",
							buttonsStyling: false,
							confirmButtonText: "Закрыть",
							customClass: {
								confirmButton: "btn btn-success"
							}
						});
					} else {
						Swal.fire({
						text: `Ошибка отправки: ${response.data.error}`,
						icon: "error",
						buttonsStyling: false,
						confirmButtonText: "Закрыть",
						customClass: {
							confirmButton: "btn btn-danger"
						}
					});

					}


				})
				.catch(function (error) {
					Swal.fire({
						text: `Ошибка отправки: ${error}`,
						icon: "error",
						buttonsStyling: false,
						confirmButtonText: "Закрыть",
						customClass: {
							confirmButton: "btn btn-danger"
						}
					});

				});
		},


	},

}

</script>

