<template>
	<transition name="modal">
		<div class="modal" v-if="isShow" @click.self="$emit('close')">
			<ShoppingList v-bind="$attrs" />
		</div>
	</transition>
</template>

<script>
	import { mapGetters } from "vuex";
	import ShoppingList from "@/components/ShoppingList.vue";
	export default {
		name: "Modal",
		inheritAttrs: false,
		components: { ShoppingList },
		computed: {
			...mapGetters({
				isShow: "shouldShowModal",
			}),
		},
		created() {
			window.addEventListener("keyup", this.onEscapeKeyUp);
		},
		methods: {
			onEscapeKeyUp(e) {
				if (e.which === 27 && this.isShow === true) {
					document.querySelector("body").style.overflow = "auto";
					this.$store.dispatch("toggleShowModal", false);
				}
			},
		},
	};
</script>

<style lang="scss" scoped>
	.modal {
		position: fixed;
		top: 0;
		left: 0;
		z-index: 1;
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		transition: opacity 0.5s ease-in-out;
		background-color: rgba(0, 0, 0, 0.527);
	}
	.modal-enter,
	.modal-leave-active {
		opacity: 0;
		& .cart {
			transform: scale(1.1);
		}
	}
</style>
