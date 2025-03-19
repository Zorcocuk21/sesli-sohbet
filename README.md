# sesli-sohbet
A WebRTC-based voice chat application for real-time communication.
import React, { useRef, useState } from "react";

const VoiceChat = () => {
  const localAudioRef = useRef(null);
  const remoteAudioRef = useRef(null);
  const [peerConnection, setPeerConnection] = useState(null);
  const servers = { iceServers: [{ urls: "stun:stun.l.google.com:19302" }] };

  const startCall = async () => {
    const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
    localAudioRef.current.srcObject = stream;
    
    const pc = new RTCPeerConnection(servers);
    setPeerConnection(pc);
    
    stream.getTracks().forEach(track => pc.addTrack(track, stream));
    
    pc.ontrack = (event) => {
      remoteAudioRef.current.srcObject = event.streams[0];
    };
  };

  return (
    <div className="p-5 bg-gray-800 text-white rounded-lg text-center">
      <h1 className="text-2xl font-bold">Sesli Sohbet</h1>
      <p>
        Daha fazla bilgi iÃ§in <a href="https://www.sesliortam.com" className="text-blue-400">sesli sohbet sitemizi</a> ziyaret edin.
      </p>
      <button onClick={startCall} className="bg-blue-500 px-4 py-2 rounded mt-4">
        Sohbete BaÅŸla
      </button>
      <div className="mt-4">
        <audio ref={localAudioRef} autoPlay controls />
        <audio ref={remoteAudioRef} autoPlay controls />
      </div>
    </div>
  );
};

export default VoiceChat;
