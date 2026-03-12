# Multi-Agent-AI-for-cybersecurity-Idea
AI agents monitor cybersecurity areas, detect threats, communicate with master agent, share knowledge, learn new attacks, and autonomously prevent incidents.

import { useState, useEffect, useRef } from "react";

const agents = [
  { id: "master", name: "MASTER AGENT", short: "NEXUS", icon: "⬡", color: "#00ff9d", desc: "Central orchestrator — coordinates all domain agents, aggregates intelligence, and manages cross-domain learning.", x: 50, y: 50, ring: 0 },
  { id: "network", name: "Network Security", short: "NET-SEC", icon: "🌐", color: "#00cfff", desc: "Monitors traffic, detects intrusions, prevents DDoS, man-in-the-middle, and packet injection attacks.", x: 50, y: 14, ring: 1 },
  { id: "appsec", name: "Application Security", short: "APP-SEC", icon: "💻", color: "#a78bfa", desc: "Scans code for OWASP Top 10, zero-days, SQL injection, XSS, and supply-chain vulnerabilities.", x: 82, y: 26, ring: 1 },
  { id: "infosec", name: "Information Security", short: "INFO-SEC", icon: "🔐", color: "#f472b6", desc: "Guards data at rest and in transit. Detects exfiltration, unauthorized access, and data leakage.", x: 90, y: 58, ring: 1 },
  { id: "cloud", name: "Cloud Security", short: "CLOUD-SEC", icon: "☁️", color: "#38bdf8", desc: "Secures AWS/Azure/GCP misconfigurations, IAM policies, container escapes, and serverless threats.", x: 72, y: 86, ring: 1 },
  { id: "endpoint", name: "Endpoint Security", short: "END-SEC", icon: "🖥️", color: "#fb923c", desc: "Protects laptops, phones, IoT devices from ransomware, trojans, and fileless malware.", x: 36, y: 88, ring: 1 },
  { id: "iam", name: "IAM", short: "IAM", icon: "👤", color: "#facc15", desc: "Manages authentication, MFA, privilege escalation detection, and zero-trust access control.", x: 14, y: 68, ring: 1 },
  { id: "soc", name: "Security Operations", short: "SOC", icon: "🛡️", color: "#4ade80", desc: "24/7 SIEM monitoring, alert triage, threat correlation, and real-time incident detection.", x: 10, y: 36, ring: 1 },
  { id: "forensics", name: "Digital Forensics", short: "FORENS", icon: "🔍", color: "#e879f9", desc: "Post-attack evidence collection, timeline reconstruction, and attacker attribution.", x: 26, y: 16, ring: 1 },
  { id: "incident", name: "Incident Response", short: "IR", icon: "🚨", color: "#f87171", desc: "Automated playbook execution, containment, eradication, and recovery coordination.", x: 68, y: 14, ring: 1 },
  { id: "grc", name: "GRC", short: "GRC", icon: "📜", color: "#a3e635", desc: "Compliance monitoring for GDPR, HIPAA, SOC2. Risk scoring and policy enforcement.", x: 92, y: 44, ring: 1 },
  { id: "pentest", name: "Penetration Testing", short: "PENTEST", icon: "🕵️", color: "#fb7185", desc: "Continuous red-team simulation, vulnerability scanning, and exploit chain mapping.", x: 80, y: 74, ring: 1 },
  { id: "malware", name: "Malware Analysis", short: "MAL-SEC", icon: "🦠", color: "#c084fc", desc: "Static/dynamic sandbox analysis, behavioral fingerprinting, and malware family classification.", x: 50, y: 88, ring: 1 },
  { id: "crypto", name: "Cryptography", short: "CRYPTO", icon: "🔑", color: "#67e8f9", desc: "Monitors cipher usage, detects weak encryption, quantum-safe algorithm migration.", x: 20, y: 80, ring: 1 },
  { id: "arch", name: "Security Architecture", short: "ARCH", icon: "🏗️", color: "#fbbf24", desc: "Evaluates system design, enforces secure-by-design principles, and models threats.", x: 10, y: 52, ring: 1 },
  { id: "iot", name: "IoT Security", short: "IOT-SEC", icon: "📡", color: "#34d399", desc: "Protects smart devices, firmware vulnerabilities, rogue device detection, and OT/ICS threats.", x: 20, y: 22, ring: 1 },
];

const threatTypes = [
  { type: "AI-Powered", examples: ["LLM Prompt Injection", "Deepfake Auth Bypass", "Adversarial ML Evasion", "AI-Generated Phishing"], badge: "AI" },
  { type: "Traditional", examples: ["SQL Injection", "Ransomware", "DDoS Flood", "Zero-Day Exploit"], badge: "TRD" },
];

const steps = [
  {
    phase: "Phase 1", title: "Foundation & Infrastructure",
    color: "#00ff9d",
    items: [
      "Set up a central orchestration platform (Apache Kafka or RabbitMQ for agent messaging)",
      "Deploy a vector database (Pinecone / Weaviate) for shared threat intelligence memory",
      "Provision compute: GPU servers or cloud (AWS/GCP) for LLM inference",
      "Install base LLM backbone: fine-tune open-source model (Llama 3 / Mistral) on cybersecurity datasets (CVE, MITRE ATT&CK)",
      "Establish secure inter-agent API layer (gRPC + mTLS encryption)"
    ]
  },
  {
    phase: "Phase 2", title: "Domain Agent Development",
    color: "#00cfff",
    items: [
      "Build 15 specialized agents — one per cybersecurity domain",
      "Each agent has: a Perception Module (sensors/logs), Reasoning Engine (LLM), Action Module (response), and Memory (RAG store)",
      "Train each on domain-specific datasets: network pcaps, malware samples, CVE databases, compliance frameworks",
      "Implement tool-calling: each agent can invoke APIs, run scripts, query SIEM, call VirusTotal, Shodan, etc.",
      "Add self-evaluation loop: agents rate their own detection confidence and flag uncertainty"
    ]
  },
  {
    phase: "Phase 3", title: "Master Agent (NEXUS) Design",
    color: "#a78bfa",
    items: [
      "Build NEXUS as a meta-agent that receives summaries from all 15 domain agents",
      "Implement a threat correlation engine: cross-domain attack chain detection",
      "Design natural language reporting: NEXUS generates human-readable incident reports in English",
      "Create a shared knowledge graph (Neo4j) linking threats, TTPs, and domains",
      "Build a dashboard UI for SOC analysts showing real-time agent activity"
    ]
  },
  {
    phase: "Phase 4", title: "Continuous Learning & Adaptation",
    color: "#f472b6",
    items: [
      "Implement federated learning: agents share anonymized threat patterns without raw data",
      "Build a Reinforcement Learning from Human Feedback (RLHF) loop for analyst corrections",
      "Add novelty detection: when an unknown attack is seen, agents flag and quarantine, then retrain",
      "Deploy automated retraining pipelines (MLflow + Kubeflow) triggered by new threat intel",
      "Integrate live threat feeds: MITRE ATT&CK, CISA KEV, OTX, Shodan, VirusTotal"
    ]
  },
  {
    phase: "Phase 5", title: "Human Transparency & Control",
    color: "#fb923c",
    items: [
      "All agent decisions logged in plain English with confidence scores and reasoning chains",
      "Analyst approval required for high-impact actions (blocking IPs, isolating systems)",
      "Audit trail: every action timestamped, reversible, and explainable",
      "Real-time chat interface: analysts can query any agent in natural language",
      "Weekly AI-generated threat briefings and learning summaries"
    ]
  },
  {
    phase: "Phase 6", title: "Testing & Deployment",
    color: "#facc15",
    items: [
      "Red team the agents: run simulated attacks to validate detection and response",
      "Staged rollout: shadow mode first (observe only), then active response",
      "Canary deployment: test new agent versions against 5% of traffic before full rollout",
      "Disaster recovery: agent state snapshots, hot-standby replicas",
      "Compliance validation: ensure system meets SOC 2, ISO 27001 requirements"
    ]
  }
];

const techStack = [
  { category: "LLM / AI Core", items: ["Llama 3 / Mistral (fine-tuned)", "LangChain / LangGraph", "OpenAI GPT-4 (optional)", "Hugging Face Transformers"], color: "#00ff9d" },
  { category: "Agent Orchestration", items: ["Apache Kafka (messaging)", "CrewAI / AutoGen (agent framework)", "gRPC (inter-agent comms)", "Kubernetes (scaling)"], color: "#00cfff" },
  { category: "Memory & Knowledge", items: ["Pinecone / Weaviate (vector DB)", "Neo4j (knowledge graph)", "Redis (fast cache)", "Elasticsearch (log store)"], color: "#a78bfa" },
  { category: "Security Tools", items: ["Zeek / Suricata (network)", "YARA (malware rules)", "OpenVAS (vulnerability)", "Velociraptor (endpoint)"], color: "#f472b6" },
  { category: "Threat Intelligence", items: ["MITRE ATT&CK API", "CISA KEV Feed", "VirusTotal API", "Shodan API"], color: "#fb923c" },
  { category: "Infrastructure", items: ["Docker / Kubernetes", "MLflow (model tracking)", "Grafana (monitoring)", "HashiCorp Vault (secrets)"], color: "#facc15" },
];

const commsLog = [
  { from: "NET-SEC", to: "MASTER", time: "00:00:01", msg: "Detected port scan from 185.220.x.x — 4,200 SYN packets/sec across 3,000 ports. Confidence: 97%. Flagging as reconnaissance phase.", severity: "HIGH" },
  { from: "MASTER", to: "IAM", time: "00:00:02", msg: "Cross-checking: did any user authenticate from IP range 185.220.x.x in last 72 hours?", severity: "INFO" },
  { from: "IAM", to: "MASTER", time: "00:00:04", msg: "Negative. No authenticated sessions from that range. Two failed MFA attempts on admin account detected 6 hours prior. Possible correlation.", severity: "WARN" },
  { from: "MASTER", to: "SOC", time: "00:00:05", msg: "Escalating to active incident. Combining port scan + failed MFA = pre-attack pattern. Initiating IR playbook Alpha-7.", severity: "CRITICAL" },
  { from: "SOC", to: "MASTER", time: "00:00:07", msg: "Playbook Alpha-7 activated. Blocking IP range at firewall. Analyst notification sent. Awaiting human approval for account lockdown.", severity: "ACTION" },
  { from: "MASTER", to: "ALL", time: "00:00:09", msg: "Broadcasting threat signature to all agents. Update your detection models. New pattern: coordinated recon + credential stuffing combo attack.", severity: "LEARN" },
  { from: "MALWARE", to: "MASTER", time: "00:00:12", msg: "Cross-referencing with known APT toolkits. Behavioral signature matches Lazarus Group lateral movement pattern. Confidence: 84%.", severity: "HIGH" },
  { from: "MASTER", to: "HUMAN", time: "00:00:14", msg: "INCIDENT REPORT #IR-2024-0847: Possible APT intrusion attempt detected. Network reconnaissance combined with credential attack. All systems on alert. Human analyst review required. Recommended action: Account lockdown + full packet capture.", severity: "CRITICAL" },
];

export default function CyberSecDashboard() {
  const [activeTab, setActiveTab] = useState("architecture");
  const [selectedAgent, setSelectedAgent] = useState(null);
  const [logIndex, setLogIndex] = useState(0);
  const [visibleLogs, setVisibleLogs] = useState([]);
  const [pulse, setPulse] = useState(false);
  const logRef = useRef(null);

  useEffect(() => {
    const interval = setInterval(() => {
      setPulse(p => !p);
    }, 2000);
    return () => clearInterval(interval);
  }, []);

  useEffect(() => {
    if (activeTab === "comms") {
      setVisibleLogs([]);
      setLogIndex(0);
      let i = 0;
      const timer = setInterval(() => {
        if (i < commsLog.length) {
          setVisibleLogs(prev => [...prev, commsLog[i]]);
          i++;
        } else {
          clearInterval(timer);
        }
      }, 900);
      return () => clearInterval(timer);
    }
  }, [activeTab]);

  useEffect(() => {
    if (logRef.current) logRef.current.scrollTop = logRef.current.scrollHeight;
  }, [visibleLogs]);

  const masterAgent = agents[0];
  const domainAgents = agents.slice(1);

  const getSeverityStyle = (s) => {
    const map = {
      CRITICAL: { bg: "#ff000022", border: "#ff4444", text: "#ff6666" },
      HIGH: { bg: "#ff660022", border: "#ff6600", text: "#ff8833" },
      WARN: { bg: "#ffcc0022", border: "#ffcc00", text: "#ffdd44" },
      INFO: { bg: "#00cfff22", border: "#00cfff", text: "#66dfff" },
      ACTION: { bg: "#a78bfa22", border: "#a78bfa", text: "#c4b5fd" },
      LEARN: { bg: "#00ff9d22", border: "#00ff9d", text: "#4fffba" },
    };
    return map[s] || map.INFO;
  };

  return (
    <div style={{
      minHeight: "100vh", background: "#030712",
      fontFamily: "'JetBrains Mono', 'Fira Code', monospace",
      color: "#e2e8f0", padding: "0"
    }}>
      {/* Header */}
      <div style={{
        borderBottom: "1px solid #00ff9d33",
        background: "linear-gradient(90deg, #030712 0%, #0a1628 50%, #030712 100%)",
        padding: "20px 40px", display: "flex", alignItems: "center", justifyContent: "space-between"
      }}>
        <div style={{ display: "flex", alignItems: "center", gap: 16 }}>
          <div style={{
            width: 44, height: 44, borderRadius: "50%",
            border: "2px solid #00ff9d",
            display: "flex", alignItems: "center", justifyContent: "center",
            fontSize: 20, boxShadow: "0 0 20px #00ff9d55",
            animation: "spin 8s linear infinite"
          }}>⬡</div>
          <div>
            <div style={{ fontSize: 20, fontWeight: 700, color: "#00ff9d", letterSpacing: 3 }}>NEXUS CYBERDEFENSE AI</div>
            <div style={{ fontSize: 11, color: "#64748b", letterSpacing: 2 }}>MULTI-AGENT THREAT INTELLIGENCE SYSTEM</div>
          </div>
        </div>
        <div style={{ display: "flex", gap: 24, alignItems: "center" }}>
          {["THREAT LEVEL", "AGENTS ONLINE", "EVENTS/SEC"].map((label, i) => (
            <div key={i} style={{ textAlign: "center" }}>
              <div style={{ fontSize: 10, color: "#64748b", letterSpacing: 1 }}>{label}</div>
              <div style={{ fontSize: 18, fontWeight: 700, color: i === 0 ? "#ff6600" : "#00ff9d" }}>
                {i === 0 ? "HIGH" : i === 1 ? "15/15" : "2.4K"}
              </div>
            </div>
          ))}
          <div style={{
            padding: "6px 14px", borderRadius: 4,
            background: "#00ff9d22", border: "1px solid #00ff9d",
            color: "#00ff9d", fontSize: 11, letterSpacing: 2
          }}>● LIVE</div>
        </div>
      </div>

      {/* Tabs */}
      <div style={{
        display: "flex", gap: 0,
        borderBottom: "1px solid #1e293b",
        background: "#060d1a", padding: "0 40px"
      }}>
        {[
          { id: "architecture", label: "🗺 ARCHITECTURE" },
          { id: "agents", label: "🤖 AGENTS" },
          { id: "comms", label: "💬 LIVE COMMS" },
          { id: "blueprint", label: "📋 BUILD BLUEPRINT" },
          { id: "stack", label: "⚙️ TECH STACK" },
        ].map(tab => (
          <button key={tab.id} onClick={() => setActiveTab(tab.id)} style={{
            padding: "14px 22px", border: "none", background: "transparent",
            color: activeTab === tab.id ? "#00ff9d" : "#64748b",
            fontSize: 11, letterSpacing: 2, cursor: "pointer",
            borderBottom: activeTab === tab.id ? "2px solid #00ff9d" : "2px solid transparent",
            transition: "all 0.2s", fontFamily: "inherit"
          }}>{tab.label}</button>
        ))}
      </div>

      <div style={{ padding: "32px 40px" }}>

        {/* ARCHITECTURE TAB */}
        {activeTab === "architecture" && (
          <div>
            <div style={{ marginBottom: 24 }}>
              <h2 style={{ color: "#00ff9d", fontSize: 16, letterSpacing: 3, margin: "0 0 8px" }}>SYSTEM ARCHITECTURE</h2>
              <p style={{ color: "#64748b", fontSize: 12, margin: 0 }}>Click any agent node to see its role. All 15 domain agents connect bidirectionally to the NEXUS master agent.</p>
            </div>
            <div style={{ display: "grid", gridTemplateColumns: "1fr 360px", gap: 32 }}>
              {/* Network diagram */}
              <div style={{
                background: "#060d1a", border: "1px solid #1e293b",
                borderRadius: 12, padding: 24, position: "relative",
                minHeight: 520
              }}>
                <svg width="100%" height="520" style={{ position: "absolute", top: 0, left: 0, pointerEvents: "none" }}>
                  {domainAgents.map(agent => {
                    const cx1 = 50, cy1 = 50;
                    const cx2 = agent.x, cy2 = agent.y;
                    return (
                      <line key={agent.id}
                        x1={`${cx1}%`} y1={`${cy1}%`}
                        x2={`${cx2}%`} y2={`${cy2}%`}
                        stroke={agent.color} strokeWidth="1"
                        strokeOpacity={pulse ? 0.4 : 0.15}
                        strokeDasharray="4 6"
                        style={{ transition: "stroke-opacity 1s" }}
                      />
                    );
                  })}
                </svg>
                {/* Master node */}
                <div
                  onClick={() => setSelectedAgent(masterAgent)}
                  style={{
                    position: "absolute", left: "50%", top: "50%",
                    transform: "translate(-50%, -50%)",
                    width: 90, height: 90, borderRadius: "50%",
                    background: "radial-gradient(circle, #00ff9d22, #030712)",
                    border: `3px solid #00ff9d`,
                    boxShadow: `0 0 ${pulse ? 40 : 20}px #00ff9d66`,
                    display: "flex", flexDirection: "column",
                    alignItems: "center", justifyContent: "center",
                    cursor: "pointer", zIndex: 10,
                    transition: "box-shadow 1s"
                  }}>
                  <div style={{ fontSize: 22 }}>⬡</div>
                  <div style={{ fontSize: 9, color: "#00ff9d", letterSpacing: 1, fontWeight: 700 }}>NEXUS</div>
                </div>
                {/* Domain agent nodes */}
                {domainAgents.map(agent => (
                  <div key={agent.id}
                    onClick={() => setSelectedAgent(agent)}
                    style={{
                      position: "absolute",
                      left: `${agent.x}%`, top: `${agent.y}%`,
                      transform: "translate(-50%, -50%)",
                      width: 62, height: 62, borderRadius: "50%",
                      background: `radial-gradient(circle, ${agent.color}22, #030712)`,
                      border: `2px solid ${agent.color}`,
                      boxShadow: selectedAgent?.id === agent.id ? `0 0 24px ${agent.color}` : `0 0 8px ${agent.color}55`,
                      display: "flex", flexDirection: "column",
                      alignItems: "center", justifyContent: "center",
                      cursor: "pointer", zIndex: 5, transition: "box-shadow 0.3s"
                    }}>
                    <div style={{ fontSize: 14 }}>{agent.icon}</div>
                    <div style={{ fontSize: 7, color: agent.color, letterSpacing: 0.5, textAlign: "center", padding: "0 4px", fontWeight: 700 }}>{agent.short}</div>
                  </div>
                ))}
              </div>

              {/* Agent info panel */}
              <div style={{ display: "flex", flexDirection: "column", gap: 16 }}>
                <div style={{
                  background: "#060d1a", border: `1px solid ${selectedAgent ? selectedAgent.color : "#1e293b"}`,
                  borderRadius: 12, padding: 24, minHeight: 180,
                  boxShadow: selectedAgent ? `0 0 20px ${selectedAgent.color}22` : "none",
                  transition: "all 0.3s"
                }}>
                  {selectedAgent ? (
                    <>
                      <div style={{ display: "flex", alignItems: "center", gap: 12, marginBottom: 16 }}>
                        <div style={{ fontSize: 32 }}>{selectedAgent.icon}</div>
                        <div>
                          <div style={{ fontWeight: 700, color: selectedAgent.color, fontSize: 14, letterSpacing: 2 }}>{selectedAgent.name}</div>
                          <div style={{ fontSize: 10, color: "#64748b", letterSpacing: 1 }}>{selectedAgent.ring === 0 ? "MASTER ORCHESTRATOR" : "DOMAIN SPECIALIST AGENT"}</div>
                        </div>
                      </div>
                      <p style={{ color: "#cbd5e1", fontSize: 13, lineHeight: 1.7, margin: 0 }}>{selectedAgent.desc}</p>
                      <div style={{ marginTop: 16, display: "flex", gap: 8, flexWrap: "wrap" }}>
                        {["DETECT", "ANALYZE", "RESPOND", "LEARN"].map(tag => (
                          <span key={tag} style={{
                            padding: "3px 10px", borderRadius: 3,
                            background: `${selectedAgent.color}22`, border: `1px solid ${selectedAgent.color}66`,
                            color: selectedAgent.color, fontSize: 10, letterSpacing: 2
                          }}>{tag}</span>
                        ))}
                      </div>
                    </>
                  ) : (
                    <div style={{ color: "#334155", fontSize: 13, textAlign: "center", paddingTop: 40 }}>
                      ← Click any agent node to see details
                    </div>
                  )}
                </div>

                {/* How agents communicate */}
                <div style={{ background: "#060d1a", border: "1px solid #1e293b", borderRadius: 12, padding: 20 }}>
                  <div style={{ color: "#00ff9d", fontSize: 11, letterSpacing: 2, marginBottom: 12 }}>HOW AGENTS COMMUNICATE</div>
                  {[
                    { arrow: "Domain → NEXUS", desc: "Threat reports, anomaly alerts, detection confidence" },
                    { arrow: "NEXUS → Domain", desc: "Task assignments, threat context, retraining signals" },
                    { arrow: "Domain ↔ Domain", desc: "Peer intelligence sharing via NEXUS knowledge graph" },
                    { arrow: "NEXUS → Human", desc: "Plain-English incident reports, approval requests" },
                  ].map((item, i) => (
                    <div key={i} style={{ display: "flex", gap: 10, marginBottom: 10, alignItems: "flex-start" }}>
                      <span style={{ color: "#00ff9d", fontSize: 10, whiteSpace: "nowrap", marginTop: 1 }}>{item.arrow}</span>
                      <span style={{ color: "#64748b", fontSize: 11 }}>{item.desc}</span>
                    </div>
                  ))}
                </div>
              </div>
            </div>
          </div>
        )}

        {/* AGENTS TAB */}
        {activeTab === "agents" && (
          <div>
            <h2 style={{ color: "#00ff9d", fontSize: 16, letterSpacing: 3, margin: "0 0 8px" }}>ALL 15 DOMAIN AGENTS + MASTER</h2>
            <p style={{ color: "#64748b", fontSize: 12, margin: "0 0 24px" }}>Each agent handles AI-powered and traditional threats, self-retrains on novel attacks, and reports to NEXUS.</p>
            <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill, minmax(300px, 1fr))", gap: 16 }}>
              {agents.map(agent => (
                <div key={agent.id} style={{
                  background: "#060d1a", border: `1px solid ${agent.color}44`,
                  borderRadius: 10, padding: 18,
                  transition: "all 0.2s",
                  boxShadow: agent.ring === 0 ? `0 0 20px ${agent.color}33` : "none"
                }}>
                  <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", marginBottom: 10 }}>
                    <div style={{ display: "flex", gap: 10, alignItems: "center" }}>
                      <span style={{ fontSize: 24 }}>{agent.icon}</span>
                      <div>
                        <div style={{ color: agent.color, fontWeight: 700, fontSize: 12, letterSpacing: 1 }}>{agent.name}</div>
                        <div style={{ color: "#334155", fontSize: 10 }}>{agent.ring === 0 ? "MASTER" : "DOMAIN"} AGENT</div>
                      </div>
                    </div>
                    <div style={{
                      padding: "2px 8px", borderRadius: 3,
                      background: "#00ff9d22", border: "1px solid #00ff9d44",
                      color: "#00ff9d", fontSize: 9
                    }}>● ACTIVE</div>
                  </div>
                  <p style={{ color: "#94a3b8", fontSize: 11, lineHeight: 1.6, margin: "0 0 12px" }}>{agent.desc}</p>
                  <div style={{ borderTop: `1px solid ${agent.color}22`, paddingTop: 10, display: "flex", gap: 12 }}>
                    <div style={{ textAlign: "center" }}>
                      <div style={{ color: agent.color, fontSize: 14, fontWeight: 700 }}>{Math.floor(Math.random() * 400) + 100}</div>
                      <div style={{ color: "#334155", fontSize: 9 }}>THREATS/DAY</div>
                    </div>
                    <div style={{ textAlign: "center" }}>
                      <div style={{ color: agent.color, fontSize: 14, fontWeight: 700 }}>{(Math.random() * 3 + 96).toFixed(1)}%</div>
                      <div style={{ color: "#334155", fontSize: 9 }}>ACCURACY</div>
                    </div>
                    <div style={{ textAlign: "center" }}>
                      <div style={{ color: agent.color, fontSize: 14, fontWeight: 700 }}>{Math.floor(Math.random() * 40) + 5}ms</div>
                      <div style={{ color: "#334155", fontSize: 9 }}>RESPONSE</div>
                    </div>
                  </div>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* LIVE COMMS TAB */}
        {activeTab === "comms" && (
          <div>
            <h2 style={{ color: "#00ff9d", fontSize: 16, letterSpacing: 3, margin: "0 0 8px" }}>LIVE AGENT COMMUNICATIONS</h2>
            <p style={{ color: "#64748b", fontSize: 12, margin: "0 0 24px" }}>Real-time inter-agent messaging in plain English. Watch agents detect, escalate, and respond to an APT attack.</p>
            <div ref={logRef} style={{
              background: "#020810", border: "1px solid #1e293b",
              borderRadius: 12, padding: 24, maxHeight: 500, overflowY: "auto"
            }}>
              {visibleLogs.map((log, i) => {
                const style = getSeverityStyle(log.severity);
                return (
                  <div key={i} style={{
                    marginBottom: 16, padding: 16, borderRadius: 8,
                    background: style.bg, border: `1px solid ${style.border}44`,
                    animation: "fadeIn 0.4s ease"
                  }}>
                    <div style={{ display: "flex", gap: 12, alignItems: "center", marginBottom: 8 }}>
                      <span style={{
                        padding: "2px 8px", borderRadius: 3,
                        background: style.border + "33", border: `1px solid ${style.border}`,
                        color: style.text, fontSize: 9, letterSpacing: 2
                      }}>{log.severity}</span>
                      <span style={{ color: "#00cfff", fontSize: 11, fontWeight: 700 }}>{log.from}</span>
                      <span style={{ color: "#334155", fontSize: 10 }}>→</span>
                      <span style={{ color: "#a78bfa", fontSize: 11, fontWeight: 700 }}>{log.to}</span>
                      <span style={{ color: "#334155", fontSize: 10, marginLeft: "auto" }}>T+{log.time}</span>
                    </div>
                    <div style={{ color: "#cbd5e1", fontSize: 12, lineHeight: 1.7 }}>{log.msg}</div>
                  </div>
                );
              })}
              {visibleLogs.length < commsLog.length && (
                <div style={{ textAlign: "center", color: "#334155", fontSize: 11 }}>
                  <span style={{ animation: "blink 1s infinite" }}>● RECEIVING TRANSMISSIONS...</span>
                </div>
              )}
            </div>
            <div style={{ marginTop: 16, display: "flex", gap: 12, flexWrap: "wrap" }}>
              {Object.entries({ CRITICAL: "#ff4444", HIGH: "#ff6600", WARN: "#ffcc00", INFO: "#00cfff", ACTION: "#a78bfa", LEARN: "#00ff9d" }).map(([k, v]) => (
                <span key={k} style={{ display: "flex", alignItems: "center", gap: 6, fontSize: 10, color: v }}>
                  <span style={{ width: 8, height: 8, borderRadius: "50%", background: v, display: "inline-block" }} />
                  {k}
                </span>
              ))}
            </div>
          </div>
        )}

        {/* BLUEPRINT TAB */}
        {activeTab === "blueprint" && (
          <div>
            <h2 style={{ color: "#00ff9d", fontSize: 16, letterSpacing: 3, margin: "0 0 8px" }}>STEP-BY-STEP BUILD BLUEPRINT</h2>
            <p style={{ color: "#64748b", fontSize: 12, margin: "0 0 24px" }}>A comprehensive 6-phase plan to build the full NEXUS multi-agent cybersecurity system from scratch.</p>
            <div style={{ display: "flex", flexDirection: "column", gap: 20 }}>
              {steps.map((step, i) => (
                <div key={i} style={{
                  background: "#060d1a", border: `1px solid ${step.color}33`,
                  borderRadius: 12, padding: 24, borderLeft: `4px solid ${step.color}`
                }}>
                  <div style={{ display: "flex", alignItems: "center", gap: 12, marginBottom: 16 }}>
                    <div style={{
                      width: 36, height: 36, borderRadius: "50%",
                      background: `${step.color}22`, border: `2px solid ${step.color}`,
                      display: "flex", alignItems: "center", justifyContent: "center",
                      color: step.color, fontWeight: 700, fontSize: 14
                    }}>{i + 1}</div>
                    <div>
                      <div style={{ color: step.color, fontSize: 10, letterSpacing: 2 }}>{step.phase}</div>
                      <div style={{ color: "#e2e8f0", fontSize: 15, fontWeight: 700 }}>{step.title}</div>
                    </div>
                  </div>
                  <div style={{ display: "flex", flexDirection: "column", gap: 10 }}>
                    {step.items.map((item, j) => (
                      <div key={j} style={{ display: "flex", gap: 12, alignItems: "flex-start" }}>
                        <span style={{ color: step.color, fontSize: 14, marginTop: 1, flexShrink: 0 }}>▸</span>
                        <span style={{ color: "#94a3b8", fontSize: 13, lineHeight: 1.6 }}>{item}</span>
                      </div>
                    ))}
                  </div>
                </div>
              ))}
            </div>

            {/* Threat handling model */}
            <div style={{ marginTop: 32, background: "#060d1a", border: "1px solid #f47 2b633", borderRadius: 12, padding: 24 }}>
              <div style={{ color: "#f472b6", fontSize: 12, letterSpacing: 2, marginBottom: 16 }}>THREAT DETECTION & LEARNING CYCLE</div>
              <div style={{ display: "flex", gap: 0, alignItems: "center", flexWrap: "wrap" }}>
                {["DETECT", "ANALYZE", "CLASSIFY\n(AI or Traditional)", "RESPOND", "DOCUMENT", "RETRAIN", "BROADCAST TO ALL AGENTS"].map((step, i, arr) => (
                  <>
                    <div key={i} style={{
                      padding: "10px 16px", borderRadius: 6,
                      background: "#0a1628", border: "1px solid #1e3a5f",
                      color: "#94a3b8", fontSize: 11, textAlign: "center",
                      whiteSpace: "pre-line"
                    }}>{step}</div>
                    {i < arr.length - 1 && <div style={{ color: "#00ff9d", margin: "0 6px", fontSize: 16 }}>→</div>}
                  </>
                ))}
              </div>
            </div>
          </div>
        )}

        {/* TECH STACK TAB */}
        {activeTab === "stack" && (
          <div>
            <h2 style={{ color: "#00ff9d", fontSize: 16, letterSpacing: 3, margin: "0 0 8px" }}>TECHNOLOGY STACK & REQUIREMENTS</h2>
            <p style={{ color: "#64748b", fontSize: 12, margin: "0 0 24px" }}>Everything you need to build NEXUS. Open-source first where possible.</p>
            <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill, minmax(280px, 1fr))", gap: 16, marginBottom: 32 }}>
              {techStack.map((cat, i) => (
                <div key={i} style={{
                  background: "#060d1a", border: `1px solid ${cat.color}44`,
                  borderRadius: 10, padding: 20
                }}>
                  <div style={{ color: cat.color, fontSize: 11, letterSpacing: 2, marginBottom: 14, fontWeight: 700 }}>{cat.category}</div>
                  {cat.items.map((item, j) => (
                    <div key={j} style={{ display: "flex", alignItems: "center", gap: 8, marginBottom: 8 }}>
                      <span style={{ width: 6, height: 6, borderRadius: "50%", background: cat.color, flexShrink: 0 }} />
                      <span style={{ color: "#94a3b8", fontSize: 12 }}>{item}</span>
                    </div>
                  ))}
                </div>
              ))}
            </div>

            {/* Hardware requirements */}
            <div style={{ background: "#060d1a", border: "1px solid #1e293b", borderRadius: 12, padding: 24 }}>
              <div style={{ color: "#facc15", fontSize: 12, letterSpacing: 2, marginBottom: 16 }}>HARDWARE REQUIREMENTS</div>
              <div style={{ display: "grid", gridTemplateColumns: "repeat(3, 1fr)", gap: 16 }}>
                {[
                  { tier: "MINIMUM (Dev/Test)", specs: ["8-core CPU", "64GB RAM", "4x NVIDIA A10 GPU", "2TB NVMe SSD", "10Gbps Network"], color: "#4ade80" },
                  { tier: "PRODUCTION", specs: ["32-core CPU", "256GB RAM", "8x NVIDIA A100 GPU", "10TB SSD Cluster", "25Gbps Network"], color: "#facc15" },
                  { tier: "ENTERPRISE", specs: ["128-core cluster", "1TB+ RAM", "H100 GPU cluster", "100TB+ distributed", "100Gbps Network"], color: "#f472b6" },
                ].map((tier, i) => (
                  <div key={i} style={{
                    background: "#020810", border: `1px solid ${tier.color}44`,
                    borderRadius: 8, padding: 16
                  }}>
                    <div style={{ color: tier.color, fontSize: 10, letterSpacing: 2, marginBottom: 12 }}>{tier.tier}</div>
                    {tier.specs.map((s, j) => (
                      <div key={j} style={{ color: "#64748b", fontSize: 11, marginBottom: 6 }}>▸ {s}</div>
                    ))}
                  </div>
                ))}
              </div>
            </div>

            {/* Cost estimate */}
            <div style={{ marginTop: 16, background: "#060d1a", border: "1px solid #1e293b", borderRadius: 12, padding: 24 }}>
              <div style={{ color: "#38bdf8", fontSize: 12, letterSpacing: 2, marginBottom: 12 }}>ESTIMATED BUILD TIMELINE</div>
              <div style={{ display: "grid", gridTemplateColumns: "repeat(3, 1fr)", gap: 12 }}>
                {[
                  { phase: "MVP (1 agent)", time: "4–8 weeks", team: "2–3 engineers" },
                  { phase: "All 15 agents", time: "6–12 months", team: "8–15 engineers" },
                  { phase: "Production-ready", time: "12–18 months", team: "20+ engineers" },
                ].map((item, i) => (
                  <div key={i} style={{ textAlign: "center", padding: 16, background: "#020810", borderRadius: 8, border: "1px solid #1e293b" }}>
                    <div style={{ color: "#38bdf8", fontSize: 12, marginBottom: 6 }}>{item.phase}</div>
                    <div style={{ color: "#e2e8f0", fontSize: 20, fontWeight: 700, marginBottom: 4 }}>{item.time}</div>
                    <div style={{ color: "#64748b", fontSize: 11 }}>{item.team}</div>
                  </div>
                ))}
              </div>
            </div>
          </div>
        )}
      </div>

      <style>{`
        @keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0.3; } }
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #030712; }
        ::-webkit-scrollbar-thumb { background: #1e293b; border-radius: 3px; }
      `}</style>
    </div>
  );
}
