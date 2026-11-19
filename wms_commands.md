# Processa os jobs da fila do tenant Granja.
docker compose exec app php artisan queue:work --queue=granja-homolog

# Executa as tarefas agendadas do Laravel.
docker compose exec app php artisan schedule:work

# Monitora a quantidade de jobs pendentes a cada 2 segundos.
watch -n 2 'docker compose exec -T postgres psql -U postgres -d rex -tAc "SELECT COUNT(*) FROM jobs WHERE queue = '\''granja-homolog'\'';"'
