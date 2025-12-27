# StreamDevices

<div align="center">
  <img src="logo.png" alt="StreamDevices Logo" width="200" height="200">
  <br>
  <strong>Gerenciamento Avançado de Dispositivos de Mídia para React</strong>
  <br>
  <br>
  <a href="https://npmjs.com/package/react-media-devices" target="_blank">
    <img src="https://img.shields.io/npm/v/react-media-devices.svg?style=for-the-badge&color=blue" alt="npm version">
  </a>
  <a href="https://github.com/maikweber/react-media-devices/blob/main/LICENSE" target="_blank">
    <img src="https://img.shields.io/npm/l/react-media-devices.svg?style=for-the-badge&color=green" alt="license">
  </a>
  <a href="https://npmjs.com/package/react-media-devices" target="_blank">
    <img src="https://img.shields.io/npm/dm/react-media-devices.svg?style=for-the-badge&color=orange" alt="downloads">
  </a>
  <br>
  <a href="https://www.typescriptlang.org/" target="_blank">
    <img src="https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript">
  </a>
  <a href="https://reactjs.org/" target="_blank">
    <img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" alt="React">
  </a>
</div>

---

## 🚀 Visão Geral

O StreamDevices é uma biblioteca de hooks para React pronta para produção, projetada para simplificar as complexidades do tratamento de mídia WebRTC. Ele oferece uma interface unificada para gerenciar permissões, alternar dispositivos dinamicamente e controlar fluxos de mídia com mínimo código boilerplate.

## ✨ Funcionalidades Principais

*   **Gerenciamento Inteligente:** Acesso e troca entre câmeras, microfones e alto-falantes sem fricção.
*   **Persistência de Estado:** Integração com Zustand para gerenciamento de estado confiável entre componentes.
*   **Controle Dinâmico:** Ative/desative o mute e o vídeo instantaneamente.
*   **Auto-Permissões:** Lida com solicitações e verificações de permissões automaticamente.
*   **Otimizado para Mobile:** Suporta preferência de câmera "traseira" (environment) para dispositivos móveis.
*   **Segurança de Tipos:** Totalmente tipado com TypeScript para uma excelente experiência de desenvolvimento.

## 📦 Instalação

O StreamDevices requer `zustand` como uma dependência peer para gerenciamento de estado.

bash
# Usando npm
npm install react-media-devices zustand

# Usando yarn
yarn add react-media-devices zustand

# Usando pnpm
pnpm add react-media-devices zustand


## 💡 Exemplo de Uso

Aqui está uma implementação básica para um componente de videoconferência:

tsx
import React, { useRef, useEffect } from 'react';
import { useUserMedia } from 'react-media-devices';

export function VideoConference() {
  const videoRef = useRef<HTMLVideoElement>(null);

  const {
    activeStream,
    audioDevices,
    videoDevices,
    outputDevices,
    ready,
    muted,
    videoOff,
    toggleMute,
    toggleVideo,
    switchInput,
    switchAudioOutput,
  } = useUserMedia({
    preferEnvironmentCamera: true // Prioriza câmera traseira em mobile
  });

  // Conecta o stream de mídia ao elemento de vídeo
  useEffect(() => {
    if (videoRef.current && activeStream) {
      videoRef.current.srcObject = activeStream;
    }
  }, [activeStream]);

  if (!ready) return <div>Carregando dispositivos...</div>;

  return (
    <div className="conference-container">
      <video 
        ref={videoRef} 
        autoPlay 
        playsInline 
        muted 
        style={{ width: '100%', background: '#000' }}
      />
      
      <div className="controls">
        <button onClick={toggleMute}>
          {muted ? 'Ativar Som' : 'Mutar'}
        </button>
        <button onClick={toggleVideo}>
          {videoOff ? 'Ligar Câmera' : 'Desligar Câmera'}
        </button>
        
        {/* Seletor de Dispositivos */}
        <select 
          onChange={(e) => switchInput({ videoDeviceId: e.target.value })}
        >
          {videoDevices.map((d) => (
            <option key={d.deviceId} value={d.deviceId}>
              {d.label || 'Câmera Desconhecida'}
            </option>
          ))}
        </select>
      </div>
    </div>
  );
}


## 🛠 Referência da API

### `useUserMedia(options?)`

O hook principal para gerenciar fluxos de mídia.

**Opções:**
*   `preferEnvironmentCamera` (boolean): Se `true`, tenta selecionar a câmera traseira em dispositivos móveis.

**Retorna:**
*   `ready` (boolean): Indica se os dispositivos foram enumerados e estão prontos.
*   `activeStream` (MediaStream | null): O fluxo de mídia ativo atual.
*   `muted` (boolean): Estado atual do microfone.
*   `videoOff` (boolean): Estado atual da câmera.
*   `audioDevices` (MediaDeviceInfo[]): Lista de microfones disponíveis.
*   `videoDevices` (MediaDeviceInfo[]): Lista de câmeras disponíveis.
*   `outputDevices` (MediaDeviceInfo[]): Lista de alto-falantes disponíveis.
*   `toggleMute()`: Alterna o áudio do microfone.
*   `toggleVideo()`: Alterna o vídeo da câmera.
*   `switchInput(deviceIds)`: Alterna a câmera ou microfone pelo ID do dispositivo.
*   `switchAudioOutput(deviceId)`: Alterna a saída de áudio (requer suporte a `setSinkId`).

## 🤝 Contribuindo

Contribuições são bem-vindas! Por favor, certifique-se de seguir o estilo de código existente e incluir testes relevantes.

1.  Fork o repositório
2.  Crie uma branch de funcionalidade (`git checkout -b feature/nova-funcionalidade`)
3.  Commit suas alterações (`git commit -m 'feat: adiciona nova funcionalidade'`)
4.  Push para a branch (`git push origin feature/nova-funcionalidade`)
5.  Abra um Pull Request

## 📄 Licença

Distribuído sob a licença MIT. Veja `LICENSE` para mais informações.