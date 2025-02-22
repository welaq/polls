<!--
  - @copyright Copyright (c) 2018 René Gieling <github@dartcafe.de>
  -
  - @author René Gieling <github@dartcafe.de>
  -
  - @license GNU AGPL version 3 or any later version
  -
  - This program is free software: you can redistribute it and/or modify
  - it under the terms of the GNU Affero General Public License as
  - published by the Free Software Foundation, either version 3 of the
  - License, or (at your option) any later version.
  -
  - This program is distributed in the hope that it will be useful,
  - but WITHOUT ANY WARRANTY; without even the implied warranty of
  - MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
  - GNU Affero General Public License for more details.
  -
  - You should have received a copy of the GNU Affero General Public License
  - along with this program.  If not, see <http://www.gnu.org/licenses/>.
  -
  -->

<template>
	<div class="user-item" :class="{ type, 'condensed' : condensed }">
		<Avatar :disable-menu="disableMenu"
			:disable-tooltip="disableTooltip"
			class="user-item__avatar"
			:is-guest="$route.name === 'publicVote'"
			:menu-position="menuPosition"
			:size="iconSize"
			:show-user-status="showUserStatus"
			:user="userId"
			:display-name="name"
			:is-no-user="isNoUser" />

		<div v-if="icon" :class="['type-icon', iconClass]" />

		<div v-if="!hideNames" class="user-item__name">
			{{ name }}
		</div>
		<slot />
	</div>
</template>

<script>
import { getCurrentUser } from '@nextcloud/auth'
import { Avatar } from '@nextcloud/vue'

export default {
	name: 'UserItem',

	components: {
		Avatar,
	},

	inheritAttrs: false,

	props: {
		hideNames: {
			type: Boolean,
			default: false,
		},
		disableMenu: {
			type: Boolean,
			default: true,
		},
		disableTooltip: {
			type: Boolean,
			default: false,
		},
		menuPosition: {
			type: String,
			default: 'left',
		},
		userId: {
			type: String,
			default: undefined,
		},
		displayName: {
			type: String,
			default: '',
		},
		type: {
			type: String,
			default: 'user',
		},
		isNoUser: {
			type: Boolean,
			default: false,
		},
		icon: {
			type: Boolean,
			default: false,
		},
		iconSize: {
			type: Number,
			default: 32,
		},
		condensed: {
			type: Boolean,
			default: false,
		},
	},

	data() {
		return {
			nothidden: false,
			circleName: '',
		}
	},

	computed: {
		name() {
			if (this.displayName) {
				return this.displayName
			}
			return this.userId

		},

		showUserStatus() {
			return Boolean(getCurrentUser())
		},

		iconClass() {
			if (this.icon) {
				if (this.type === 'contact') {
					return 'icon-mail'
				} else if (this.type === 'email') {
					return 'icon-mail'
				} else if (this.type === 'external') {
					return 'icon-share'
				} else if (this.type === 'contactGroup') {
					return 'icon-group'
				} else if (this.type === 'circle') {
					return 'icon-circles'
				}
				return 'icon-' + this.type
			}
			return ''

		},

	},
}

</script>

<style lang="scss">
.user-item {
	display: flex;
	align-items: center;
	padding: 4px;
	max-width: 100%;
}

.user-item__name {
	flex: 1;
	min-width: 50px;
	color: var(--color-text-maxcontrast);
	padding-left: 8px;
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
}

.condensed {
	&.user-item {
		flex-direction: column;
		justify-content: center;
		max-width: 70px;
	}
	.user-item__name {
		font-size: 0.7em;
		text-align: center;
		width: 70px;
		max-width: 70px;
		padding: 0 4px;
	}
}

.type-icon {
	margin-left: 8px;
	background-size: 16px;
}

</style>
