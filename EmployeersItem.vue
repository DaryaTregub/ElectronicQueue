<style>
.emps_item {
	color: #5E6278;
	cursor: pointer;
}
.emps_item:hover {
	color:#50cd89;
	transition: all 0.5s;
}

.emps_item:active {
	color: #47be7d;
}

.emps_item:hover .bullet {
	background-color: #50cd89;
	transition: all 0.5s;
}

.emps_item:active .bullet {
	background-color: #47be7d;
	transition: all 0.5s;
}

.activeEmp {
	color: #47be7d;
}
.activeBullet {
	background-color: #47be7d;
}


</style>
<template>
	<li class="d-flex align-items-center py-2 emps_item" :class="{activeEmp: value.rid == activeEmp}" @click="this.$emit('clickEmp', value)" v-for="(value, key, index) in emps"
		:key="index"><span
				class="bullet me-5" :class="{activeBullet: value.rid == activeEmp}"></span><span>{{
		value.fio
				}}</span>
	</li>

</template>
<script>

export default {
	data() {
		return {

		}
	},
	props: {
		emps: null,
		activeEmp: null,
	},
	emits: {
		clickEmp: null,
		selectChange: null,
	},
	mounted() {
		this.loadFilials();

	},
	methods: {
		loadFilials() {
			this.$emit('selectChange');
		},

		selectItem(){

  }



	}

}


</script>
