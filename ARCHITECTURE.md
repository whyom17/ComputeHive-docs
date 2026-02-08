# System Architecture

This document describes the **high-level system architecture** of the Secure Decentralized GPU Renting Platform.

The goal of this architecture is to enable **secure, time-bound GPU access between untrusted parties**, while protecting both the lender’s host system and the borrower’s data.

> ⚠️ Note: This document intentionally omits infrastructure-level and security-critical implementation details.

---

## 🧩 High-Level Components

### 1. User Roles
- **Lender**: Owns a machine with a dedicated GPU
- **Borrower**: Needs temporary GPU access for compute tasks

### 2. SaaS Control Plane
- Manages listings, bookings, and session lifecycle
- Orchestrates VM creation and teardown
- Issues time-scoped access credentials

### 3. Compute Isolation Layer
- GPU is assigned to a **virtual machine (VM)** using passthrough
- Borrower connects only to the VM, never the host
- VM is destroyed after the session ends

### 4. Blockchain Escrow
- Holds renter funds during the session
- Automatically releases payment based on session outcome
- Provides immutable rental records

### 5. AI-Assisted Automation
- Suggests GPU pricing
- Assists with scheduling
- Creates calendar events for both parties

---

## 🔁 System Flow (Simplified)

1. Lender lists available GPU resources
2. Borrower books a session for a specific time window
3. Funds are locked in a blockchain escrow contract
4. Platform provisions an isolated VM with GPU passthrough
5. Borrower receives time-bound SSH access to the VM
6. Session runs within agreed limits
7. VM is destroyed after session completion
8. Escrow releases payment to the lender

---

## 🗺️ Architecture Diagram

```mermaid
flowchart TB
    subgraph Borrower
        B1[Web UI / SSH Client]
    end

    subgraph Lender
        L1[Host Machine]
        L2[GPU]
        L3[Isolated VM]
    end

    subgraph Platform
        P1[SaaS Control Plane]
        P2[AI Agents]
    end

    subgraph Blockchain
        C1[Escrow Smart Contract]
    end

    B1 -->|Booking & Session Request| P1
    P1 -->|Pricing & Scheduling| P2
    P1 -->|Provision VM| L3
    L2 -->|GPU Passthrough| L3
    B1 -->|Time-bound SSH| L3

    B1 -->|Payment| C1
    C1 -->|Escrow Release| Lender

    P1 -->|Session Status| C1
