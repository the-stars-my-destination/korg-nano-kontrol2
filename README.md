# Korg NanoKontrol2 with Overtone

__EXPERIMENTAL__ : Using NanoKontrol2 with Overtone.

To be clear all credit and hard work is that of @samaaron and @meta-ex.
I extracted this out of the Ignite project: https://github.com/meta-ex/ignite.

## Device Setup

You will need to setup your NanoKontrol2 such that software can control all the LEDS.

    1. Download KORG KONTROL Editor http://www.korg.co.uk/support/downloads/nano2_dl.php
    2. Set LED MODE to EXTERNAL

![](http://s14.postimg.org/5qot9xyq9/Korg_Kontrol_Editor_Untitled_2013_10_19_12_2.jpg)

    3. Write the scene data (this saves the change to the device)

## Project.clj

```clojure
[korg-nano-kontrol2 "0.1.0-SNAPSHOT"]
```

## Software setup

You have to pass the banks mapping and the for those banks the s/m/r 0-7 mappings.

```clojure
(require '[nano-kontrol2.core :as nk2])
(use '[nano-kontrol2.config :only [mixer-init-state]])

(def cfg
  {:synths {:s0 mixer-init-state :s1 mixer-init-state :s2 mixer-init-state :m0 mixer-init-state :m1 mixer-init-state :r0 mixer-init-state}
   :riffs  {:s0 mixer-init-state :s1 mixer-init-state :m0 mixer-init-state :m1 mixer-init-state}
   :master {:s7 mixer-init-state :m7 mixer-init-state :r7 mixer-init-state}})

(def banks
  {:master 0
   :m64 2
   :m128 4
   :riffs 8
   :synths 16})

(nk2/start! cfg banks)
```

## Control

In order to push the use of the NanoKontrol some buttons are mapped to do different things.

   * Cycle -> Enter select bank mode.
     * rewind, fastforward, stop, play and record all select a bank.
     * within a bank mode s,m & r buttons highlighted respresent further different states.

   * Marker right -> Refresh state maps
   * Marker left  -> ?
   * Marker SET   -> Force all sync

What you assign the banks to is completely up to you. One example:

```Clojure
(defn nk-bank
  "Returns the nk bank number for the specified bank key"
  [bank-k]
  (case bank-k
    :master 0    ; record button (bank 0)
    :monome64 2  ; play button (bank 2)
    :monome128 4 ; stop button (bank 4)
    :riffs 8     ; fast-forward button (bank 8)
    :synths 16)) ; rewind button (bank 16)
```
