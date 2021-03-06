<template>
	<Modal
		:show="show"
		:show-close-button="showCloseButton"
		:max-width="maxWidth"
		:persistent="persistent"
		:max-height="computedMaxHeight"
		@close="close"
	>
		<div class="c-modal--wrapper" :class="computedClasses">
			<!-- title 영역 -->
			<div class="c-modal--title-wrapper">
				<Typography type="headline5" :align="align" color="gray900">
					<slot name="title" />
				</Typography>
			</div>

			<!-- content 영역 -->
			<div class="c-modal--content-wrapper">
				<Typography type="body2" :align="align" color="gray800">
					<slot name="content" />
				</Typography>
			</div>

			<!-- button 영역 -->
			<div v-if="showActionButton" class="c-modal--action-button-wrapper">
				<Button size="large" :loading="loading" full @click="successCallback">
					{{ successMessage }}
				</Button>
			</div>
		</div>
	</Modal>
</template>

<script>
import Modal from '@/src/Components/Message/Modal/Modal';
import Typography from '@/src/Elements/Core/Typography/Typography';
import Button from '@/src/Components/Button/Button';

export const aligns = ['left', 'center', 'right'];

export default {
	name: 'BasicModal',
	props: {
		show: {
			type: Boolean,
			default: false,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
		align: {
			type: String,
			default: 'center',
			validator(value) {
				return aligns.indexOf(value) !== -1;
			},
		},
		// 버튼 로딩
		loading: {
			type: Boolean,
			default: false,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
		successCallback: {
			type: Function,
			default: () => {},
			validator(value) {
				return typeof value === 'function';
			},
		},
		successMessage: {
			type: String,
			default: '확인',
			validator(value) {
				return typeof value === 'string';
			},
		},
		showCloseButton: {
			type: Boolean,
			default: true,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
		showActionButton: {
			type: Boolean,
			default: false,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
		persistent: {
			type: Boolean,
			default: false,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
		maxWidth: {
			type: String,
			validator(value) {
				return typeof value === 'string';
			},
		},
		scroll: {
			type: Boolean,
			default: false,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
	},
	computed: {
		computedClasses() {
			return [this.computedScroll, this.computedWithButton];
		},
		computedScroll() {
			return { scroll: this.scroll };
		},
		computedWithButton() {
			return { 'with-button': this.showActionButton };
		},
		computedMaxHeight() {
			return this.isMobile ? '86vh' : '640px';
		},
	},
	methods: {
		close() {
			this.$emit('update:show', false);
		},
	},
	components: {
		Typography,
		Modal,
		Button,
	},
};
</script>

<style lang="scss" scoped>
.c-modal {
	&--wrapper {
		padding: 40px 0;
		&.scroll {
			.c-modal--content-wrapper {
				padding-bottom: 4px;
				&:after {
					content: '';
					position: absolute;
					width: 100%;
					height: 30px;
					left: 0;
					bottom: calc(40px - 16px);
					background: linear-gradient(
						180deg,
						rgba(255, 255, 255, 0) 0%,
						rgba(255, 255, 255, 0.6) 40%,
						rgba(255, 255, 255, 1) 90%
					);
				}
			}

			&.with-button {
				.c-modal--content-wrapper {
					max-height: calc(640px - 336px);
					@include pc {
						max-height: calc(640px - 207px);
					}
					&:after {
						bottom: calc(40px + 48px + 24px - 16px);
					}
				}
			}
		}
	}

	&--title-wrapper {
		padding: 0 32px 16px 32px;
	}

	&--content-wrapper {
		padding-left: 32px;
		padding-right: 32px;

		&::v-deep * + *:not(br) {
			margin-top: 8px;
		}
		max-height: calc(640px - 265px);
		overflow-y: auto;
		@include pc {
			max-height: calc(640px - 129px);
		}
	}
	&--action-button-wrapper {
		padding: 24px 32px 0 32px;
	}
}
</style>
