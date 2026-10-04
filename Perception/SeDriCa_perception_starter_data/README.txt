SeDriCa perception starter data. All examples are original and synthetic.
They are teaching cases, not evidence of real-car performance.
city/lane: three 24-frame RGB sequences, 480 x 320.
city/lane_reference.csv: true lane centers for evaluation only.
city/calibration.json: four planar point correspondences.
city/crossing: three 24-frame RGB sequences.
city/crossing_evidence.csv: noisy scores, speed, distance and V2X messages.
city/crossing_reference.csv: true crossing states for evaluation only.
race/scans.csv: five 18-frame LiDAR sequences; empty cell is a missing return.
race/sensor.json: scan angles, vehicle geometry, corridor bounds and units.
race/scene_reference.csv: obstacle positions for evaluation only.
localization/reference_scans.csv: eight candidate-place fingerprints.
localization/observed_scans.csv: a 12-step route with noisy odometry.
localization/route_graph.csv: stay/move transitions.
localization/route_reference.csv: true places for evaluation only.
Do not use reference labels within prediction functions.
