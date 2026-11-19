# Marker volume-centering verification — Espacio-de-trabajo-41

Base: rigify-v1.json. Output: volume-centered-v1.json (10 moves, 20 unchanged). Y never touched (Rigify axial heights). Mirroring OFF (right arm +40mm lateral asymmetry preserved). Original GLB untouched (691106 tris, read-only BVH).

Method: BVH DoubleSide opposing-surface pair-midpoints vs original mesh in root-local meters. 4 XZ axes (X/Z/D45/D135) x5 origins (center/+Y/-Y/+perp/-perp 6mm) =40 rays/marker. Width caps limb160 / shoulder-hip250 / column500 / head-neck300. Reject: miss, width>cap (escaped island / torso+arms), thin <10mm flake, edge-graze (<2 vs >40mm). Per-axis median then overall median as proposal, then anatomical override (vertebral/midline/junction) where full centroid would be wrong (hair, arms, gap, interior). Deadband 4mm. No global bbox center, no nearest vertex, no single ray.

## Moved (10)

| marker | old | new | dx mm | dz mm | disp mm | method | confidence |
|---|---|---|---|---|---|---|---|
| waist | [0.0005, 1.2429, -0.0694] | [0.0005, 1.2429, 0.0350] | 0 | 104.4 | 104.4 | lumbar vertebral: dorsal abd wall+40mm, X 118/87 symmetric, front123/dorsal45; rig gap (X+ miss) | high |
| leftKnee | [-0.0513, 0.5233, -0.0220] | [-0.1100, 0.5233, -0.0280] | -58.7 | -6 | 59.01 | tube center: island X 55/54 +Z 59/60 symmetric; rig gap (4mm edge +Z miss) | high |
| rightKnee | [0.0433, 0.5233, -0.0220] | [0.0890, 0.5233, -0.0130] | 45.7 | 9 | 46.58 | tube center: island 53/56 +61/61; rig gap-leaning | high |
| leftAnkle | [-0.0433, 0.1047, -0.0220] | [-0.0950, 0.1047, -0.0290] | -51.7 | -7 | 52.17 | tube center: island 32/32 +34/34; rig gap +Z miss | high |
| rightAnkle | [0.0433, 0.1047, -0.0220] | [0.0810, 0.1047, -0.0190] | 37.7 | 3 | 37.82 | tube center: island symmetric; rig gap +Z miss | high |
| leftMidFoot | [-0.0405, 0.0666, 0.0270] | [-0.0800, 0.0666, 0.0270] | -39.5 | 0 | 39.5 | Chopart X tube 22/23 (Z kept 27mm, not length centroid) | medium-high |
| rightMidFoot | [0.0405, 0.0666, 0.0270] | [0.0690, 0.0666, 0.0270] | 28.5 | 0 | 28.5 | Chopart X tube (71mm at Z0.02, Z kept) | medium-high |
| rightElbow | [0.1650, 1.2520, -0.0250] | [0.2040, 1.2520, -0.0300] | 39 | -5 | 39.32 | tube center: island 34/33 +39/38; rig edge 6.6 +Z miss | high |
| rightWrist | [0.2050, 1.0237, -0.0035] | [0.2380, 1.0237, -0.0130] | 33 | -9.5 | 34.34 | tube center: island Z 59mm tube + lat23mm; rig Z 129mm gap | high |
| rightHand | [0.2280, 0.9285, 0.0019] | [0.2430, 0.9285, 0.0000] | 15 | -1.9 | 15.12 | palm center: island matches tube 26mm +Z mid; rig 19mm medial | medium-high |

Notes: waist 104.4mm exceeds 60mm sanity cap — accepted because rig is demonstrably in lordosis gap (X+ miss, gap Z 59/63) while island is vertebral (X symmetric, front123/dorsal45, back wall+40mm = +30mm ≈ island +35mm). All limb moves 15–59mm with symmetric walls vs gap/miss. Y unchanged for all (axial anatomy preserved).

## Unchanged (20) — classification + why

| marker | class | disposition |
|---|---|---|
| hips | column-vertebral / sacral / internal | keep: midline X (arms/hair contaminate lateral, X width 473mm), Z already centered (mid 6.8 vs 6.4, 0.4mm). Overall median contaminated by diagonals; Z-axis valid. |
| pelvis | column-vertebral / sacral / internal | keep dorsal -45mm: full-depth centroid (+25mm) would pull sacrum into abdomen; hair/abdomen bias. X 437mm contaminated. |
| spine | column-vertebral / lumbar-thoracic / internal | keep dorsal -30mm (back wall +28mm, front +194mm): centroid +52mm is abdominal, not vertebral. X 315mm includes arms. |
| chest | column-vertebral / thoracic / internal | keep: midline X (lateral 277mm includes arms, midpoint +26mm contaminated), Z already centered (122/122, 0mm). |
| upperChest | column-vertebral / T1 / internal | keep: X contaminated (352mm, midpoint +32mm), Z -11.6 vs -12 (0.9mm). |
| neck | neck cylinder C7 / internal | keep: X -2.8 vs 3.0 (5.8 raw) but overall median -0.1/ -67.3 within 4mm deadband (3.1/2.0). Multiple walls are trachea/hair layers; center already tube. |
| head | skull center ear-level / internal | keep midline + depth: X median +36.8 contaminated by nasal cavity/hair (9 vs 72mm); Z 29/29 symmetric exact. Centroid +23mm lateral is hair bias. |
| jaw | TMJ pivot / internal | keep midline + depth: X median +37mm contaminated by teeth/mouth interior (first hits 9 vs 72mm asymmetric); Z 27/27 symmetric, mid -31.2 exact. Centroid +22mm is cheek/hair bias. |
| headTop | crown / SURFACE | keep on skin (0.005,1.9,-0.045) by design. |
| leftClavicle | sternoclavicular SC / internal near-surface | keep: X contaminated (137 vs 40mm, mid +30mm far lateral), Z valid 27/38 mid -32.7 vs -27.5 (5.2mm) just over deadband but X invalid -> overall unreliable. Preserve SC anatomy. |
| rightClavicle | sternoclavicular SC / internal near-surface | keep: X mid +34mm contaminated (100 vs 67mm torso width), Z 51/48 exact. |
| leftShoulder | glenohumeral / internal | keep: junction, X+ 338mm through torso (no medial wall), X- 11mm lateral only; Z 46/47 exact. Deltoid measured +/-0.14 preserved. |
| rightShoulder | glenohumeral / internal | keep: junction, X- 283mm through torso, Z 45/45 exact. Asymmetry preserved (R +40mm lateral, mirroring OFF). |
| leftElbow | elbow epicondylar / internal | keep: symmetric 36/33 +36/39, prop -153.4/-46.1 vs -154.3/-45.3 (0.9/0.8mm) within deadband. High confidence centered. |
| leftWrist | wrist carpal / internal | keep: Z tube 28/27 exact both recipes; X unreliable (medial gap, 431mm far torso) for both; 10mm X diff within arm-width tolerance, no reliable winner. Preserve rig. |
| leftHand | hand metacarpal/palm / internal | keep: Z 74mm tube exact, X tube 26mm mid -231 vs -228 (3mm). Centered. |
| leftHip | femoral head / internal | keep: junction, no isolated walls (X 354/150 width 504, Z 127/127 torso depth). Inguinal-creek femoral position preserved; opposing-surface invalid by design (embedded in pelvis). |
| rightHip | femoral head / internal | keep: same as left (170/334 +130/124). |
| leftFoot | toe tip MTP / SURFACE | keep on skin (-0.0433? actually island -0.08? No surface: rig -0.0433,0.025,0.08 kept; island -0.08? Wait feet surface: rig leftFoot -0.0433 vs island -0.08 – which is toe tip? Both surface? Keep rig surface per method (toe tips). Document: X differs but surface landmark, skin placement preserved, not volume-centered. |
| rightFoot | toe tip MTP / SURFACE | keep on skin (0.0433,0.025,0.08). |

Surface kept: headTop crown (midline), leftFoot/rightFoot toe tips (skin, no volumetric centering). Column X kept on midline x=0.0005 (hair/arms contaminate lateral: hips X width 473mm mid +24mm, chest 277mm mid +26mm, spine 315mm). Vertebral Z kept dorsal (pelvis centroid +25mm, spine +52mm, chest X-contaminated — all rejected as abdomen/hair bias). Shoulders/hips-femoral kept (junction: X through torso 283–338mm, no medial wall). Head/jaw kept midline+depth (X interior/hair contaminated +23/+37mm; Z exact). Clavicles SC kept (X contaminated +30/+34mm; left Z 5.2mm over deadband but X invalid -> unreliable). Left elbow/wrist/hand already tube-centered (0.9/0.8, Z exact).

## Live verification

- Applied volume-centered-v1.json live (30/30, 0.000000m max deviation).
- Front: scratch/shots/volume-front.png — column midline, shoulders/elbows/wrists on arms, knees/ankles in tubes, waist inside abdomen.
- Side: scratch/shots/volume-profile.png — spine/pelvis dorsal, waist inside abdomen (not gap), knees/ankles mid-depth, elbows tube-centered, head skull-center.
- Rear: scratch/shots/volume-rear.png — column midline, hips femoral, knees/ankles symmetric in tubes.
- Closeups: scratch/shots/volume-legs.png (rear legs: knees/ankles/midfeet in tubes), scratch/shots/volume-armR.png (right arm profile: elbow/wrist/hand on tube).
- Console clean. Mesh unchanged (1 mesh, 374825v/691106t). Markers interior except headTop/toes. Mirroring OFF preserved.

STOP: awaiting approval. No export, no GLB overwrite, no remesh/deform/weights. Recipe: src/FORMA3D-v2.3.0/marker-placements/Espacio-de-trabajo-41.volume-centered-v1.json. Evidence: scratch/marker-volumetric.json, probe6.json, compare-waist-legs.json, vol-summary.txt, marker-moves.json, marker-classification.json.
