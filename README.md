# StreamDevices

<div align="center">
  <img src="logo.png" alt="StreamDevices Logo" width="200" height="200">
  <br>
  <strong>Advanced Media Device Management for React</strong>
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

## 🚀 Overview

StreamDevices is a production-ready React hook library designed to simplify the complexities of WebRTC media handling. It provides a unified interface for managing permissions, switching devices dynamically, and controlling media streams with minimal boilerplate.

## ✨ Core Features

*   **Smart Device Management:** Seamlessly access and switch between cameras, microphones, and speakers.
*   **State Persistence:** Built-in Zustand integration for reliable state management across components.
*   **Dynamic Control:** Toggle mute/unmute and video on/off instantly.
*   **Auto-Permissions:** Handles permission requests and verification automatically.
*   **Mobile Optimized:** Supports "environment" camera preference for mobile devices.
*   **Type Safety:** Fully typed with TypeScript for an excellent developer experience.

## 📦 Installation

StreamDevices requires `zustand` as a peer dependency for state management.

bash
# Using npm
npm install react-media-devices zustand

# Using yarn
yarn add react-media-devices zustand

# Using pnpm
pnpm add react-media-devices zustand


## 💡 Usage Example

Here is a basic implementation for a video conference component:

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
    preferEnvironmentCamera: true // Prioritizes rear camera on mobile
  });

  // Connect the media stream to the video element
  useEffect(() => {
    if (videoRef.current && activeStream) {
      videoRef.current.srcObject = activeStream;
    }
  }, [activeStream]);

  if (!ready) return <div>Loading devices...</div>;

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
          {muted ? 'Unmute' : 'Mute'}
        </button>
        <button onClick={toggleVideo}>
          {videoOff ? 'Camera On' : 'Camera Off'}
        </button>
        
        {/* Device Switcher */}
        <select 
          onChange={(e) => switchInput({ videoDeviceId: e.target.value })}
        >
          {videoDevices.map((d) => (
            <option key={d.deviceId} value={d.deviceId}>
              {d.label || 'Unknown Camera'}
            </option>
          ))}
        </select>
      </div>
    </div>
  );
}


## 🛠 API Reference

### `useUserMedia(options?)`

The primary hook for managing media streams.

**Options:**
*   `preferEnvironmentCamera` (boolean): If `true`, attempts to select the rear camera on mobile devices.

**Returns:**
*   `ready` (boolean): Indicates if devices are enumerated and ready.
*   `activeStream` (MediaStream | null): The current active media stream.
*   `muted` (boolean): Current microphone state.
*   `videoOff` (boolean): Current camera state.
*   `audioDevices` (MediaDeviceInfo[]): List of available microphones.
*   `videoDevices` (MediaDeviceInfo[]): List of available cameras.
*   `outputDevices` (MediaDeviceInfo[]): List of available speakers.
*   `toggleMute()`: Toggles microphone audio.
*   `toggleVideo()`: Toggles camera video.
*   `switchInput(deviceIds)`: Switches the camera or microphone by Device ID.
*   `switchAudioOutput(deviceId)`: Switches the speaker output (requires `setSinkId` support).

## 🤝 Contributing

Contributions are welcome! Please ensure you follow the existing code style and include relevant tests.

1.  Fork the repository
2.  Create a feature branch (`git checkout -b feature/amazing-feature`)
3.  Commit your changes (`git commit -m 'feat: add amazing feature'`)
4.  Push to the branch (`git push origin feature/amazing-feature`)
5.  Open a Pull Request

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.