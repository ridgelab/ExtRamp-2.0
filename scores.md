## A description of the score file columns

* **header**: the sequence header
* **CDS length**: the sequence length in nucleotides
* **ramp length**: the ramp length in nucleotides. If the sequence does not have a ramp, the length is -1
* **non-ramp min mean**: the z-score minimum sliding window mean of the non-ramp region. If no ramp is present, the non-ramp region is the last 92% of the sequence
* **ramp min mean**: the z-score minimum sliding window mean of the ramp region. If no ramp is present, the ramp region is the first 8% of the sequence
* **ramp strength**: the ramp strength score, a measure of the steepness of the ramp compared to the rest of the sequence
    * **non-ramp min mean** - **ramp min mean**
* **num ramp region status changing mutations**: the number of point mutations in the ramp region that change ramp status
* **num non-ramp region status changing mutations**: the number of point mutations in the non-ramp region that change ramp status
* **num reasonably valid ramp mutations**: the number of non-stop point mutations in the ramp region that 1) increase local efficiency when a ramp is present or 2) decrease local efficiency when a ramp is absent
* **num reasonably valid non-ramp mutations**: the number of non-stop point mutations in the non-ramp region that 1) decrease local efficiency when a ramp is present or 2) increase local efficiency when a ramp is absent
* **ramp robustness**: the ramp robustness score, the fraction of reasonably valid point mutations that don't change ramp status. The score is multiplied by -1 if no ramp is present: 
    * multiplier * (1 - ((**num ramp region status changing mutations** + **num non-ramp region status changing mutations**) / (**num reasonably valid ramp mutations** + **num reasonably valid non-ramp mutations**)))