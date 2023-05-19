<script lang="ts">
	import JsonViewer from './JsonViewer.svelte';
	import type { ValueType } from './api';

	export let label: string;
	export let value: object | string | number | null;
	export let expanded = false;
	export let summaryLength = 30;

	const getValueType = (val: unknown): ValueType => {
		if (typeof val === 'string') return 'string';
		if (typeof val === 'number') return 'number';
		if (val == null) return 'null';
		if (Array.isArray(val)) return 'array';
		return 'object';
	};

	const isNestedType = (valType: ValueType) => valType === 'array' || valType === 'object';
	$: valueType = getValueType(value);
	$: isNested = isNestedType(valueType);

	const toggle = () => {
		expanded = !expanded;
	};

	export function changeExpand(expand: boolean) {
		expanded = expand;
	}

	export function changeExpandNested(expand: boolean) {
		jsonViewer?.changeExpandAll(expand);
		changeExpand(expand);
	}

	const getDetail = (value: any, valType: ValueType | 'object' | 'array') => {
		if (valType === 'array') {
			return arrayDetail(value);
		} else {
			return objectDetail(value);
		}
	};

	const objectDetail = (obj: object) => {
		let str = '{ ';
		for (const entry of Object.entries(obj)) {
			const newStr = `${entry[0]}: ${detailItemValue(entry[1])}, `;
			if (newStr.length + str.length > summaryLength) return str + ' ... }';
			str += newStr;
		}
		return str.substring(0, str.length - 2) + ' }';
	};

	const arrayDetail = (arr: unknown[]) => {
		let str = '[ ';
		for (const item of arr) {
			const newStr = `${detailItemValue(item)}, `;
			if (newStr.length + str.length > summaryLength) return str + ' ... ]';
			str += newStr;
		}
		return str.substring(0, str.length - 2) + ' ]';
	};

	const detailItemValue = (val: unknown) => {
		if (Array.isArray(val)) {
			return '[ ... ]';
		} else if (val == null) {
			return null;
		} else if (typeof val === 'object') {
			return '{ ... }';
		} else if (typeof val === 'string') {
			return `"${val}"`;
		}
		return val;
	};
	const showNull = (val: unknown) => {
		if (val == null) return 'null';
		return val;
	};

	let jsonViewer: JsonViewer | undefined;

	const getValue = () => {
		return value as any;
	};
</script>

<div class="first-line">
	<!-- svelte-ignore a11y-click-events-have-key-events -->
	<div class:toggle={isNested} on:click={toggle}>
		{#if isNested}
			<div class="arrow" class:expanded>▶</div>
		{/if}
		<div class="label">{label}</div>
	</div>
	{#if isNested}
		{#if !expanded}
			<span class="detail">{getDetail(value, valueType)}</span>
		{/if}
	{:else}
		<span class={valueType}>
			{showNull(value)}
		</span>
	{/if}
</div>
{#if isNested}
	<div class="json-view" style:display={expanded ? 'block' : 'none'}>
		<JsonViewer bind:this={jsonViewer} data={getValue()} />
	</div>
{/if}

<style>
	.toggle {
		display: flex;
		gap: var(--first-line-gap);
		cursor: pointer;
		user-select: none;
	}

	.first-line {
		display: flex;
		gap: var(--first-line-gap);
	}

	.arrow {
		transition: transform 100ms;
		font-weight: 400;
	}
	.arrow.expanded {
		transform: rotate(90deg);
	}
	.label {
		&::after {
			content: ':';
		}

		&:first-child {
			padding-left: 1.32em;
		}
	}
	.json-view {
		padding-left: calc(1.32em - 1px);
	}

	.label,
	.arrow {
		color: var(--json-label, #6fb3d2);
	}

	.string {
		color: var(--json-string, #a3eea0);
	}

	.number {
		color: var(--json-number, #d19a66);
	}

	.null {
		color: var(--json-null, #df9cf3);
	}

	.detail {
		color: var(--json-detail, rgb(222 175 143 / 90%));
	}

	:global(body).dark {
		--json-label: #6fb3d2;
		--json-string: #a3eea0;
		--json-number: #d19a66;
		--json-null: #df9cf3;
		--json-detail: rgb(222 175 143 / 90%);
	}

	:global(body).light {
		--json-label: #1a0f80;
		--json-string: #1e901a;
		--json-number: #c06e22;
		--json-null: #ac21d7;
		--json-detail: rgba(192, 115, 64, 0.9);
	}
</style>
