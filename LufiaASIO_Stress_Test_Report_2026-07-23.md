# LufiaASIO Public Stress Test Report

**Report date:** July 23, 2026  
**Product:** LufiaASIO 1.2.8 Alpha  
**Test category:** Digital loopback, data integrity, format compatibility, buffer sweep, and sustained operation  
**Report edition:** Consumer results summary

## 1. Executive summary

LufiaASIO was evaluated with a two-input, two-output BKSV physical digital audio loopback bridge developed on XMOS. The test program covered WASAPI Shared, WASAPI Exclusive, native ASIO, WDM-KS, and MME across representative sample rates and ASIO host buffers from 512 to 1,048,576 frames.

The principal results were:

- WASAPI Exclusive and native ASIO passed exact full-32-bit pseudorandom-data loopback at 768 kHz across the complete six-size buffer sweep.
- WDM-KS passed exact full-32-bit loopback at all eight sample rates exposed by the test device from 44.1 to 384 kHz.
- Four low-buffer sustained-operation cases completed with zero discontinuities, underflows, and overflows.
- MME opened at all 14 rates exposed by the test device from 8 to 384 kHz and passed the applicable signal and THD+N checks.
- Digital-silence tests through MME returned zero non-zero input bytes at 44.1, 48, and 384 kHz.
- WASAPI Shared reported only its 384 kHz Windows Mix Format and correctly rejected 1536 kHz instead of exposing the device's Exclusive-mode rate set, This is the expected behavior.
- The 2 × 2 WASAPI Exclusive/native-ASIO route and measurement-processing matrix passed 16 of 16 streams in both 32-bit and 64-bit host environments.

The validated pure WASAPI Exclusive, native ASIO, WDM-KS, and representative MME routes showed no data-loss indicators in the sustained cases reported below.

## 2. Test setup

| Item | Test configuration |
|---|---|
| Audio interface | Hottinger Brüel & Kjær (BKSV) USB audio device |
| Hardware arrangement | Physical digital output-to-input loopback |
| Platform basis | XMOS |
| Active channels | 2 input × 2 output |
| Host architectures | 32-bit and 64-bit |
| Primary high-rate test | 768 kHz |
| Largest verified host buffer | 1,048,576 frames |
| Smallest verified host buffer | 512 frames |
| Integrity signal | Full-width 32-bit pseudorandom sequence |
| Quality signal | 1 kHz sine at −2 dBFS |
| Silence criterion | Zero non-zero captured bytes after stabilization |
| Sustained-operation criteria | Zero discontinuity, underflow, and overflow counters |

The BKSV bridge remained in a physical digital loopback configuration throughout the reported signal-integrity tests. Results describe this specific test system and do not establish a guaranteed specification for unrelated audio hardware.

## 3. Coverage summary

| Audio mode | Tested rate coverage | Buffer coverage | Primary result |
|---|---|---|---|
| WASAPI Shared | 384 kHz Mix Format | 512 to 1,048,576 | Loopback and THD+N checks passed; 1536 kHz correctly rejected |
| WASAPI Exclusive | 768 kHz | 512 to 1,048,576 | Exact full-32-bit loopback passed |
| Native ASIO | 768 kHz | 512 to 1,048,576 | Exact full-32-bit loopback passed |
| WDM-KS | 44.1 to 384 kHz, eight exposed rates | 512 to 1,048,576 at 384 kHz | Exact full-32-bit loopback passed |
| MME | 8 to 384 kHz, 14 exposed rates | 512 to 1,048,576 at 384 kHz | All exposed rates opened; signal, silence, and THD+N checks passed where applied |

The six buffer sizes used for the full sweep were:

| Buffer index | ASIO host buffer |
|---:|---:|
| 1 | 512 frames |
| 2 | 2,048 frames |
| 3 | 16,384 frames |
| 4 | 65,536 frames |
| 5 | 524,288 frames |
| 6 | 1,048,576 frames |

## 4. Full-32-bit data-integrity results

The integrity test transmitted a full-width 32-bit pseudorandom sequence through the physical digital loopback. Captured data was aligned to the transmitted sequence before all complete comparable samples were checked.

| Route | Rate | Coverage | Result |
|---|---:|---|---|
| WASAPI Exclusive input ↔ output | 768 kHz | All six buffer sizes | Pass — exact 32-bit samples |
| Native ASIO input ↔ output | 768 kHz | All six buffer sizes | Pass — exact 32-bit samples |
| WDM-KS input ↔ output | 44.1, 48, 88.2, 96, 176.4, 192, 352.8, and 384 kHz | All eight device-supported rates | Pass — exact 32-bit samples |
| WDM-KS input ↔ output | 384 kHz | All six buffer sizes | Pass — exact 32-bit samples; zero overflow |

Representative 32-bit-host checks also passed at:

- WASAPI Exclusive: 768 kHz / 512 frames
- Native ASIO: 768 kHz / 512 frames
- WDM-KS: 384 kHz / 512 frames
- MME: 384 kHz / 512 frames under its applicable non-bit-perfect criteria

No 1536 kHz bit-perfect result is claimed by this report. The BKSV hardware configuration used for the high-rate integrity tests accepted 768 kHz for WASAPI Exclusive and native ASIO. LufiaASIO can request rates up to 1536 kHz from a WASAPI Exclusive endpoint, but a rate is exposed only when the connected device and its driver accept it.

## 5. THD+N and digital-silence results

THD+N was measured through the physical digital loopback using a 1 kHz, −2 dBFS test signal. Non-bit-perfect modes were included only for compatibility verification and are not intended as performance benchmarks.

| Mode | Rate coverage | Observed THD+N | Result |
|---|---:|---:|---|
| WDM-KS | Eight exposed rates from 44.1 to 384 kHz | Approximately −150.80 to −159.75 dB | Pass |
| MME | 384 kHz | Approximately −159.81 dB | Pass |
| WASAPI Shared | 384 kHz | Approximately −159.71 dB | Pass |

### Digital silence

| Mode | Rate | Non-zero captured bytes | Result |
|---|---:|---:|---|
| MME | 44.1 kHz | 0 | Pass |
| MME | 48 kHz | 0 | Pass |
| MME | 384 kHz | 0 | Pass |

MME and WASAPI Shared may use Windows format conversion. Their THD+N and silence results demonstrate the quality observed on this test path but do not convert those modes into bit-perfect transports.

## 6. Low-buffer sustained-operation results

Four representative routes were operated at the minimum 512-frame host buffer while a non-zero physical loopback signal was confirmed. Raw frame totals and public diagnostic counters are shown below.

| Mode | Rate / buffer | Duration | Input frames | Output frames | Discontinuities | Input / output underflows | Input / output overflows | Result |
|---|---|---|---:|---:|---|---|---|---|
| Native ASIO | 768 kHz / 512 | 120 s | 92,160,000 | 92,160,000 | 0 | 0 / 0 | 0 / 0 | Pass |
| WASAPI Exclusive | 768 kHz / 512 | 120 s | 92,160,000 | 92,163,840 | 0 | 0 / 0 | 0 / 0 | Pass |
| WDM-KS | 384 kHz / 512 | 60 s | 23,035,904 | 23,035,904 | 0 | 0 / 0 | 0 / 0 | Pass |
| MME | 384 kHz / 512 | 60 s | 23,048,192 | 23,048,192 | 0 | 0 / 0 | 0 / 0 | Pass |

The frame totals are reported exactly as recorded. The WASAPI Exclusive case counted 3,840 more output frames than input frames—5 ms at 768 kHz—but showed no discontinuity, underflow, or overflow indication. The separate sample-by-sample integrity test in Section 4 provides the bit-perfect result.

An additional 32-bit-host MME check at 384 kHz / 512 frames ran for 15 seconds and processed 5,771,264 input frames and 5,771,264 output frames. All reported discontinuity, underflow, and overflow counters remained zero.

## 7. Measurement-processing matrix

The measurement-processing check combined the two eligible transport modes in every input/output pairing:

| Input mode | Output mode |
|---|---|
| WASAPI Exclusive | WASAPI Exclusive |
| WASAPI Exclusive | Native ASIO |
| Native ASIO | WASAPI Exclusive |
| Native ASIO | Native ASIO |

Each route was exercised in four states:

1. Processing bypassed
2. C2 THD Compensation active
3. Fixed Notch active
4. Adaptive Notch active

| Host environment | Completed streams | Passed streams | Result |
|---|---:|---:|---|
| 64-bit | 16 | 16 | Pass |
| 32-bit | 16 | 16 | Pass |
| **Total** | **32** | **32** | **Pass** |

This matrix verifies routing and activation behavior for the listed processing states. It does not prescribe coefficients, Notch settings, or calibration values for another device.

## 8. WASAPI Shared rate-reporting result

The BKSV endpoint's Windows Mix Format during this test was 384 kHz. In WASAPI Shared mode, LufiaASIO:

- reported 384 kHz to the ASIO host;
- rejected a 1536 kHz request;
- did not substitute the endpoint's WASAPI Exclusive capability list;
- completed loopback and THD+N checks at 384 kHz for all six host-buffer sizes.

This is the expected behavior. WASAPI Shared is governed by the active Windows Mix Format, while rates accepted through WASAPI Exclusive are a separate capability set.

## 9. Result statement

Within the tested BKSV digital-loopback scope:

- pure WASAPI Exclusive and pure native ASIO sustained operation passed at 768 kHz / 512 frames for 120 seconds with zero reported discontinuities, underflows, or overflows;
- both modes passed exact full-32-bit loopback from 512 through 1,048,576 host frames at 768 kHz;
- WDM-KS passed exact full-32-bit loopback across all eight exposed rates from 44.1 through 384 kHz;
- representative MME operation passed signal, silence, THD+N, rate-opening, buffer-sweep, and sustained-operation checks under non-bit-perfect criteria;
- the 32-bit and 64-bit measurement-processing matrices passed all 32 streams;
- WASAPI Shared correctly remained limited to the 384 kHz Windows Mix Format;

This Alpha report is an engineering evaluation summary, not a calibration certificate, regulatory test, or guaranteed performance specification. Users should validate their own device, rate, channel layout, clocking, and measurement-processing profile against a trusted reference before production or compliance use.

---

**Test platform note:** Results were obtained with a BKSV physical audio I/O loopback bridge developed on XMOS. Product and company names belong to their respective owners and are used only to identify the evaluated interoperability environment.
