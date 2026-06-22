# halo2-fv
Formal verification of Halo2

## Checklist

### 1. Rust and Lean verifier equivalence (fingerprint)

- [x] Gate first: audit the Halo2/IPA verifier's error paths. Enumerate every condition it can error on, and classify each one as either input-side (a malformed or non-canonical proof string, which is fine) or an internal mid-computation branch (which invalidates the fingerprint approach). If any internal branch exists, halt and re-evaluate.
- [x] For each surviving error condition, decide whether it has negligible probability (such as a 2-adic challenge or a hash collision, which can be dismissed under assumption) or whether it breaks an assumption.
- [x] Refactor the Rust verifier so it consumes the proof string as opaque field and group elements, never inspecting them and only performing symbolic operations like inverting and combining. The output is a symbolic multiexponentiation over the SRS and the proof elements.
- [x] Pick the fingerprint form: either a polynomial-coefficient formula or a random evaluation using random field and group elements.
- [x] Run the refactored Rust verifier on a fake or random proof and capture the fingerprint.
- [x] Pin down the soundness bound of the random-evaluation (Schwartz–Zippel) argument.
- [x] Have AI write an independent Lean reimplementation of the verifier.
- [x] Run the same fingerprint on the Lean version and compare. A match implies equivalence with high probability.

### 2. SNARK soundness (in Lean)

- [x] State the finite-field arithmetic assumptions explicitly (this layer remains an unverified trust assumption).
- [x] Write the interactive protocol in Lean.
- [x] Prove special soundness (construct the extractor).
- [x] Apply Fiat-Shamir to collapse the interactive protocol into a non-interactive verifier.
- [x] Fingerprint the Fiat-Shamir-derived verifier against the Rust verifier (reusing step 1). This confirms the proven-sound verifier is the deployed one.
- [x] Extract the witness from the special-sound SNARK.
- [ ] Have AI extract the full witness-from-relation result into a high-level relation (dependency on section 3).
- [ ] Have a human check that the relation and security notions are the right ones (dependency on section 3).

### 3. Circuit and verifying key side (Daira's flow)

- [ ] Via reductions, prove that the statement matches the circuit.
- [ ] Compile the circuit in Lean to produce a verifying key.
- [ ] Compare the Lean-compiled verifying key to the Rust verifying key.
- [ ] Prove the verifying key encodes the correct things for both soundness and correctness.
