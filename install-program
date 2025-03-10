$computadores = @("PC1", "PC2", "PC3")  # Substitua pelos nomes ou IPs das máquinas
$installerUrl = "https://172.16.107.11/epi_win_live_installer.exe"
$installerPath = "C:\Temp\epi_win_live_installer.exe"

foreach ($pc in $computadores) {
    Write-Host "===================================="
    Write-Host "▶ Instalando em: $pc"
    
    Invoke-Command -ComputerName $pc -ScriptBlock {
        param ($installerUrl, $installerPath)

        Write-Host "🔹 Criando diretório C:\Temp (se necessário)..."
        if (!(Test-Path "C:\Temp")) {
            New-Item -Path "C:\Temp" -ItemType Directory | Out-Null
        }

        Write-Host "🔹 Baixando instalador..."
        Invoke-WebRequest -Uri $installerUrl -OutFile $installerPath

        Write-Host "🔹 Executando instalação..."
        Start-Process -FilePath $installerPath -ArgumentList "/silent /norestart" -Wait -PassThru

        Write-Host "🔹 Limpando arquivos temporários..."
        Remove-Item -Path $installerPath -Force

        Write-Host "✅ Instalação concluída em $env:COMPUTERNAME!"
        Write-Host "🔄 Reiniciando em 10 segundos..."
        Start-Sleep -Seconds 10
        Restart-Computer -Force

    } -ArgumentList $installerUrl, $installerPath -AsJob

    Write-Host "🚀 Instalação iniciada em: $pc"
    Write-Host "===================================="
}
