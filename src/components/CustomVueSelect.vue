<!-- eslint-disable no-undef -->
<!-- eslint-disable no-nested-ternary -->
<!-- eslint-disable space-before-blocks -->
<!-- eslint-disable object-curly-newline -->
<!-- eslint-disable vue/singleline-html-element-content-newline -->
<!-- eslint-disable vue/max-attributes-per-line -->
<!-- eslint-disable vue/html-self-closing -->
<!-- eslint-disable vue/html-indent -->

<template>
	<div
		:id="id"
		v-click-outside="closeDropdown"
		class="vue-select"
		:class="{ 'is-open': isOpen }"
		:tabindex="tabindex"
	>
		<div class="select-body" @click="isOpen = !isOpen">
			<div class="selected-value text-nowrap" :title="getOptionValue(selected)">
				{{ getOptionValue(selected) }}
				<span v-if="!selected && placeholder" class="placeholder">
					{{ placeholder }}
				</span>
			</div>
			<div class="indicator-icon">
				<img alt="bill-debt" draggable="false" width="23" />
			</div>
		</div>
		<transition name="fade">
			<div v-show="isOpen" class="dropdown">
				<slot name="header" />
				<div v-if="!options.length" class="d-flex justify-content-center p-2">
					<!-- <puff-loading :style="{ color: 'var(--primary)' }" /> -->
				</div>
				<ul>
					<li
						v-for="(item, index) in options"
						:key="index"
						@click="preventDefaultClickEvent ? null : onChange(item)"
					>
						<slot
							name="option"
							:item="item"
							:value="getOptionValue(item)"
							:click="onChange.bind(this, item)"
						>
							{{ getOptionValue(item) }}
						</slot>
					</li>
				</ul>
				<slot name="footer" />
			</div>
		</transition>
	</div>
</template>

<script>
// import PuffLoading from "../svg-icons/PuffLoading.vue";

export default {
	components: {
		// PuffLoading,
	},
	props: {
		id: {
			type: String,
			required: false,
		},
		value: {
			type: [Object, Array, String, Number],
			required: false,
			default: null,
		},
		options: {
			type: Array,
			required: true,
		},
		basedOn: {
			type: String,
			default: null,
		},
		targetKey: {
			type: String,
			required: false,
		},
		preventDefaultClickEvent: {
			type: Boolean,
			default: false,
		},
		tabindex: {
			type: Number,
			required: false,
			default: 0,
		},
		placeholder: {
			type: String,
			default: "",
		},
	},
	data() {
		return {
			selected: this.value.value || null,
			isOpen: false,
		};
	},
	computed: {
		hasOptionSlot() {
			return !!this.$slots.option;
		},
	},
	mounted() {
		this.$emit("input", this.selected);
	},
	methods: {
		getOptionValue(anOption) {
			if (anOption === Object(anOption)) return anOption[this.basedOn];
			return anOption;
		},
		onChange(value) {
			this.selected = value;

			this.$emit("input", {
				value: this.selected,
				targetValue: this.targetKey ? this.selected[this.targetKey] : null,
			});

			this.isOpen = false;
		},
		closeDropdown() {
			this.isOpen = false;
		},
		textTrim(text, length) {
			if (typeof text === "string") {
				return text.length > length ? `... ${text.substring(0, length)}` : text;
			}
			return text;
		},
	},
};
</script>

<style lang="scss" scoped>
// @import "@core/scss/base/bootstrap-extended/_variables.scss";

$border-radius: 10px;
$black: #000;
$cvs-box-shadow: 0 4px 15px 0 rgba($black, 0.3);
$primary: #fff;
.vue-select {
	margin-top: 0.5rem;
	width: 100%;
	position: relative;
}

.vue-select > .select-body {
	direction: rtl;
	display: flex;
	flex-direction: row-reverse;
	justify-content: space-between;
	border: solid 1px rgba(0, 0, 0, 0.2);
	border-radius: $border-radius;
	align-items: center;
	transition: box-shadow 200ms linear;
}

.vue-select.is-open .select-body {
	box-shadow: $cvs-box-shadow;
}

.vue-select .selected-value {
	height: 3rem;
	padding: 0.9rem 1rem;
	overflow: hidden;
	text-overflow: ellipsis;
	direction: ltr;
}

.placeholder {
	color: rgba(0, 0, 0, 0.6);
}

.vue-select.is-open .indicator-icon {
	transform: rotate(180deg);
}
.vue-select .indicator-icon {
	transition: transform 200ms linear;
	padding: 0 0.5rem;
}

.vue-select > .dropdown {
	z-index: 10;
	padding: 1rem;
	box-shadow: $cvs-box-shadow;
	position: absolute;
	background-color: white;
	border-radius: $border-radius;
	left: 0;
	right: 0;
	max-height: 25vh;
	overflow: auto;
}

.vue-select ul li {
	width: 100%;
	padding: 0 0 0.7rem 0;
	border-bottom: solid 0.01rem rgba(0, 0, 0, 0.1);
	margin-bottom: 0.7rem;
	color: rgba(0, 0, 0, 0.8);
	font-size: 1.1rem;
}

.vue-select ul li:last-child {
	border: none;
	margin: 0;
}

ul li {
	list-style: none;
}

ul {
	margin: 0;
	padding: 0;
}
</style>
