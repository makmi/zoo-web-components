<svelte:options tag="zoo-input"></svelte:options>
<div class="box {labelposition}">
	<zoo-input-label class="input-label" valid="{valid}" labeltext="{labeltext}">
	</zoo-input-label>
	<zoo-link class="input-link" href="{linkhref}" target="{linktarget}" type="grey" text="{linktext}" textalign="right">
	</zoo-link>
	<span class="input-slot {nopadding ? 'no-padding': ''}">
		<slot bind:this={_inputSlot} name="inputelement"></slot>
		{#if valid}
		<slot name="inputicon"></slot>
		{/if} {#if !valid}
		<svg class="error-triangle" width="22" height="22" viewBox="0 0 24 24"><path d="M12 18a1.125 1.125 0 1 1 .001 2.25A1.125 1.125 0 0 1 12 18H12zm.75-2.25a.75.75 0 1 1-1.5 0v-7.5a.75.75 0 1 1 1.5 0v7.5zm1.544-14.32l9.473 19.297A2.271 2.271 0 0 1 21.728 24H2.272a2.271 2.271 0 0 1-2.04-3.272L9.707 1.429a2.556 2.556 0 0 1 4.588 0zm-2.76.178c-.21.103-.379.273-.482.482L1.58 21.39a.771.771 0 0 0 .693 1.111h19.456a.771.771 0 0 0 .693-1.112L12.948 2.091a1.056 1.056 0 0 0-1.414-.483z"/></svg>
		{/if}
	</span>
	<zoo-input-info class="input-info" valid="{valid}" inputerrormsg="{inputerrormsg}" infotext="{infotext}">
	</zoo-input-info>
</div>

<style type='text/scss'>
	@import "variables";
	@import "input";

	.error-triangle {
		animation: hideshow 0.5s ease;
		position: absolute;
		right: 0;
		top: 0;
		padding: 11px;
		color: $error-text-color;
		pointer-events: none;

		& > path {
			fill: $error-text-color;
		}
	}

	::slotted(input), 
	::slotted(textarea) {
		width: 100%;
		font-size: 14px;
		line-height: 20px;
		padding: 13px 35px 13px 15px;
		margin: 0;
		border: 1px solid;
		border-color: $border-color;
		border-radius: 3px;
		color: $matterhorn;
		outline: none;
		box-sizing: border-box;
		text-overflow: ellipsis;
		-moz-appearance: textfield;
	}

	::slotted(input)::-webkit-inner-spin-button {
		-webkit-appearance: none;
		margin: 0;
	}

	::slotted(input)::-webkit-outer-spin-button {
		-webkit-appearance: none;
		margin: 0;
	}

	::slotted(input::placeholder),
	::slotted(textarea::placeholder) {
		color: $placeholder-color;
		opacity: 1;
	}

	::slotted(input:disabled),
	::slotted(textarea:disabled) {
		border-color: #e6e6e6;
		background-color: #f2f3f4;
		color: #97999c;
		cursor: not-allowed;
	}

	::slotted(input:focus),
	::slotted(textarea:focus) {
		border: 2px solid;
		padding: 12px 34px 12px 14px;
	}

	::slotted(input.error),
	::slotted(textarea.error) {
		transition: border-color 0.3s ease;
		border: 2px solid;
		padding: 12px 34px 12px 14px;
		border-color: $error-text-color;
	}

	::slotted(input[type='date']), ::slotted(input[type='time']) {
		-webkit-appearance: none;
	}

	.input-slot.no-padding ::slotted(input) {
		padding: 0;
	}
	@keyframes hideshow {
		0% { opacity: 0; }

		100% { opacity: 1; }
	} 
</style>

<script>
	import { beforeUpdate, onMount } from 'svelte';

	export let labelposition = "top";
	export let labeltext = "";
	export let linktext = "";
	export let linkhref = "";
	export let linktarget = "about:blank";
	export let inputerrormsg = "";
	export let infotext = "";
	export let valid = true;
	export let nopadding = false;
	let _slottedInput;
	let _prevValid;
	let _inputSlot;

	beforeUpdate(() => {
		if (valid != _prevValid) {
			_prevValid = valid;
			changeValidState(valid);
		}
	});
	  
	onMount(() => {
		_inputSlot.addEventListener("slotchange", () => {
			let nodes = _inputSlot.assignedNodes();
			_slottedInput = nodes[0];
			changeValidState(valid);
	    });
	});

	const changeValidState = (valid) => {
		if (_slottedInput) {
			if (!valid) {
				_slottedInput.classList.add('error');
			} else if (valid) {
				_slottedInput.classList.remove('error');
			}
		}
	}
</script>