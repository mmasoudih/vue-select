
<template>
	<div
		:id="id"
		v-click-outside="closeDropdown"
		class="vue-select"
		:class="{ 'is-open': isOpen }"
		:tabindex="tabindex"
	>
		<div class="select-body" @click="isOpen = !isOpen">
			<div class="selected-value">
				<span
					v-for="selectedItem in selectedItems"
					:key="getOptionValue(selectedItem)"
					class="value-tag"
				>
					{{ getOptionValue(selectedItem) }}
				</span>
				<span v-if="!selectedItems.length && placeholder" class="placeholder">
					{{ placeholder }}
				</span>
			</div>
			<div class="indicator-icon">
				<img
					
					alt="bill-debt"
					draggable="false"
					width="23"
				/>
			</div>
		</div>
		<transition name="fade">
			<div v-show="isOpen" class="dropdown">
				<slot name="header" />
				<ul>
					<li
						v-for="(item, index) in options"
						:key="index"
						@click="preventDefaultClickEvent ? null : onChange(item, $event)"
					>
						<slot
							name="option"
							:item="item"
							:value="getOptionValue(item)"
							:click="onChange.bind(this, item)"
						>
							<b-form-checkbox class="not-clickable">
								{{ getOptionValue(item) }}
							</b-form-checkbox>
						</slot>
					</li>
				</ul>
				<slot name="footer" />
			</div>
		</transition>
	</div>
</template>

<script>
import { BFormCheckbox } from "bootstrap-vue";

export default {
	components: {
		BFormCheckbox,
	},
	model: {
		prop: "selected",
		event: "input",
	},
	props: {
		id: {
			type: String,
			required: false,
		},
		defaultSelected: {
			type: Array,
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
		placeholder: {
			type: String,
			default: "",
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
	},
	data() {
		return {
			selectedItems: this.defaultSelected || [],
			isOpen: false,
		};
	},
	computed: {
		hasOptionSlot() {
			return !!this.$slots.option;
		},
	},
	mounted() {
		this.$emit("input", this.selectedItems);
		
	},
	methods: {
		getOptionValue(anOption) {
			if (anOption === Object(anOption)) return anOption[this.basedOn];
			return anOption;
		},
		onChange(value, event) {
			const valueIndex = this.selectedItems.findIndex(
				(item) => this.getOptionValue(value) === this.getOptionValue(item)
			);

			if (valueIndex !== -1) {
				const newItems = this.selectedItems.filter((item, index) => index !== valueIndex);
				this.selectedItems = newItems;
			} else this.selectedItems = [...this.selectedItems, value];

			if (this.targetKey) {
				this.$emit(
					"input",
					this.selectedItems.map((item) => item[this.targetKey])
				);
			} else {
				this.$emit("input", this.selectedItems);
			}

			const checkboxNode = event.target.querySelector("input[type='checkbox']");
			checkboxNode.checked = !checkboxNode.checked;

			this.closeDropdown();
		},
		closeDropdown() {
			this.isOpen = false;
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
	width: 100%;
	min-width: 0;
	position: relative;
	background-color: white;
}

.vue-select > .select-body {
	width: 100%;
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
	overflow: auto;
	display: flex;
	align-items: center;
	padding: 0.5rem 0.7rem;
}

.vue-select .value-tag {
	white-space: nowrap;
	background-color: $primary;
	color: white;
	padding: 0.1rem 0.5rem;
	border-radius: $border-radius;
	margin-right: 0.5rem;
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
	padding: 0.5rem 1rem;
	box-shadow: $cvs-box-shadow;
	position: absolute;
	background-color: white;
	border-radius: $border-radius;
	left: 0;
	right: 0;
	max-height: 250px;
	overflow: scroll;
}

.vue-select ul li {
	width: 100%;
	padding: 0.7rem 0;
	border-bottom: solid 0.01rem rgba(0, 0, 0, 0.1);
	color: rgba(0, 0, 0, 0.8);
	font-size: 1.1rem;
}

.vue-select ul li:last-child {
	border: none;
	margin: 0;
}

.placeholder {
	color: rgba(0, 0, 0, 0.6);
}

.not-clickable {
	pointer-events: none;
}

ul li {
	list-style: none;
}

ul {
	margin: 0;
	padding: 0;
}
</style>
