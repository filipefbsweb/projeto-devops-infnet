<template>
	<div class="container">
		<h1>Sistema de Cadastro de Usuários</h1>
		<div class="form-container">
			<h2>Cadastrar Novo Usuário</h2>
			<form @submit.prevent="adicionarUsuario">
				<div class="form-group">
					<label for="nome">Nome:</label>
					<input type="text" id="nome" v-model="novoUsuario.nome" required />
				</div>

				<div class="form-group">
					<label for="email">Email:</label>
					<input type="email" id="email" v-model="novoUsuario.email" required />
				</div>

				<div class="form-group">
					<label for="telefone">Telefone:</label>
					<input type="tel" id="telefone" v-model="novoUsuario.telefone" />
				</div>

				<button type="submit" class="btn-cadastrar">Cadastrar</button>
			</form>
		</div>

		<div class="users-container">
			<h2>Usuários Cadastrados</h2>
			<div v-if="usuarios.length === 0" class="no-users">
				Nenhum usuário cadastrado.
			</div>
			<table v-else class="users-table">
				<thead>
					<tr>
						<th>Nome</th>
						<th>Email</th>
						<th>Telefone</th>
						<th>Ações</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="(usuario, index) in usuarios" :key="index">
						<td>{{ usuario.nome }}</td>
						<td>{{ usuario.email }}</td>
						<td>{{ usuario.telefone }}</td>
						<td>
							<button class="btn-excluir" @click="excluirUsuario(index)">
								Excluir
							</button>
						</td>
					</tr>
				</tbody>
			</table>
		</div>
	</div>
</template>

<script>
import { Counter, Histogram } from "prometheus-client-js";

export default {
	name: "App",
	data() {
		return {
			usuarios: [],
			novoUsuario: {
				nome: "",
				email: "",
				telefone: "",
			},
		};
	},
	methods: {
		adicionarUsuario() {
			this.usuarios.push({ ...this.novoUsuario });
			// Limpa o formulário
			this.novoUsuario = {
				nome: "",
				email: "",
				telefone: "",
			};
		},
		excluirUsuario(index) {
			this.usuarios.splice(index, 1);
		},
	},
	mounted() {
		const pageViews = new Counter({
			name: "vue_page_views_total",
			help: "Total de visualizações de página",
		});

		const loadTime = new Histogram({
			name: "vue_page_load_time_seconds",
			help: "Tempo de carregamento da página em segundos",
			buckets: [0.1, 0.5, 1, 2, 5],
		});

		pageViews.inc();
		const endTime = performance.now();
		loadTime.observe((endTime - window.initialLoadTime) / 1000);
	},
};
</script>
