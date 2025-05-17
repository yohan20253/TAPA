local Players = game:GetService("Players")

-- Quantidade de dano a aplicar
local DANO_COMPARTILHADO = 20

-- Função para aplicar dano em todos os jogadores, exceto o original
local function aplicarDanoEmTodos(origem)
	for _, jogador in ipairs(Players:GetPlayers()) do
		if jogador ~= origem and jogador.Character and jogador.Character:FindFirstChild("Humanoid") then
			local humanoid = jogador.Character.Humanoid
			humanoid:TakeDamage(DANO_COMPARTILHADO)
		end
	end
end

-- Monitorar cada jogador
Players.PlayerAdded:Connect(function(jogador)
	jogador.CharacterAdded:Connect(function(personagem)
		local humanoid = personagem:WaitForChild("Humanoid")

		-- Detecta quando esse jogador sofre dano
		local vidaAnterior = humanoid.Health

		humanoid.HealthChanged:Connect(function(vidaAtual)
			if vidaAtual < vidaAnterior then
				aplicarDanoEmTodos(jogador)
			end
			vidaAnterior = vidaAtual
		end)
	end)
end)
