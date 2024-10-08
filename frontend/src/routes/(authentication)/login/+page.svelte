<script lang="ts">
	import type { PageData } from './$types';
	import CisoLogo from '$lib/assets/ciso.svg';
	import { loginSchema } from '$lib/utils/schemas';
	import TextField from '$lib/components/Forms/TextField.svelte';
	import SuperForm from '$lib/components/Forms/Form.svelte';
	import Typewriter from 'svelte-typewriter';

	import * as m from '$paraglide/messages.js';
	import { zod } from 'sveltekit-superforms/adapters';
	import { redirectToProvider } from '$lib/allauth.js';

	export let data: PageData;
</script>

<div class="relative h-screen w-screen bg-slate-200">
	<div class="absolute top-5 left-5">
		<div class="flex flex-row w-full space-x-4 pb-3">
			<img class="c" height="200" width="200" src={CisoLogo} alt="Ciso-assistant icon" />
		</div>
	</div>
	<div class="absolute top-1/2 left-1/2 w-full transform -translate-x-1/2 -translate-y-1/2">
		<div class="flex flex-row w-full pr-8">
			<div id="hellothere" class="flex flex-col justify-center items-center w-3/5 text-gray-900">
				<Typewriter mode="loopOnce" cursor={false} interval={50}>
					<div class="text-2xl unstyled text-center pb-4">
						<span class="text-2xl text-center">{m.helloThere()} </span>
						<span> {m.thisIsCisoAssistant()} </span>
					</div>
				</Typewriter>
				<Typewriter mode="cascade" cursor={false} interval={45} delay={5000}>
					<div class="text-2xl unstyled text-center">
						<span> {m.yourStreamlined()} </span>
						<span class="font-black"> {m.oneStopShop()} </span>
						<span> {m.forComplianceRiskManagement()} </span>
					</div>
				</Typewriter>
			</div>
			<div class="flex justify-center pr-5 items-center space-y-4 w-2/5">
				<div class="flex flex-col w-3/4 p-10 rounded-lg shadow-lg bg-white bg-opacity-[.90]">
					<div data-testid="login" class="flex flex-col w-full items-center space-y-4">
						<div class="bg-primary-300 px-6 py-5 rounded-full text-3xl">
							<i class="fa-solid fa-right-to-bracket" />
						</div>
						<h3
							class="font-bold leading-tight tracking-tight md:text-2xl bg-gradient-to-r from-pink-500 to-violet-600 bg-clip-text text-transparent"
						>
							{m.logIntoYourAccount()}
						</h3>
						<p class="text-center text-gray-600 text-sm">
							{m.youNeedToLogIn()}
						</p>
						<div class="w-full">
							<!-- SuperForm with dataType 'form' -->
							<SuperForm
								class="flex flex-col space-y-3"
								data={data?.form}
								dataType="form"
								let:form
								validators={zod(loginSchema)}
							>
								<TextField type="email" {form} field="username" label={m.email()} />
								<TextField type="password" {form} field="password" label={m.password()} />
								<div class="flex flex-row justify-end">
									<a
										href="/password-reset"
										class="flex items-center space-x-2 text-primary-800 hover:text-primary-600"
										data-testid="forgot-password-btn"
									>
										<p class="">{m.forgtPassword()}?</p>
									</a>
								</div>
								<p class="">
									<button
										class="btn variant-filled-primary font-semibold w-full"
										data-testid="login-btn"
										type="submit">{m.login()}</button
									>
								</p>
							</SuperForm>
						</div>
						{#if data.SSOInfo.is_enabled}
							<div class="flex items-center justify-center w-full space-x-2">
								<hr class="w-64 items-center bg-gray-200 border-0" />
								<span class="flex items-center text-gray-600 text-sm">{m.or()}</span>
								<hr class="w-64 items-center bg-gray-200 border-0" />
							</div>
							<button
								class="btn bg-gradient-to-l from-violet-800 to-violet-400 text-white font-semibold w-1/2"
								on:click={() =>
									redirectToProvider(data.SSOInfo.sp_entity_id, data.SSOInfo.callback_url, 'login')}
								>{m.loginSSO()}</button
							>
						{/if}
					</div>
				</div>
			</div>
		</div>
	</div>
</div>
