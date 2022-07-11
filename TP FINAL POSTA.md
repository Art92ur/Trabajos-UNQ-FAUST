//Percusión -8bits-
import("stdfaust.lib");


 frecuencia = nentry("freq", 300, 300, 300, 1);
 velocity = nentry("gain", 1, 0, 1, 0.01);
 gateDeNotas = button("gate");


volumen = vslider("master", 0.5, 0, 1, 0.01);

//Redoblante
attackRedo = 0.025;
decayRedo = 0.125;
sustainRedo = 0.01;
releaseRedo = 0.25;

redo = no.pink_noise*  en.adsr(attackRedo, decayRedo, sustainRedo, releaseRedo, gateDeNotas) * velocity * volumen * condicionalRedo;

ingresoRedo = ba.hz2midikey(frecuencia);
condicionalRedo = ingresoRedo==40;

//Hi-Hat

attackHiHat = 0.001;
decayHiHat  = 0.1;
sustainHiHat  = 0;
releaseHiHat  = 0;

hihatCerrado = no.noise*  en.adsr(attackHiHat , decayHiHat , sustainHiHat , releaseHiHat , gateDeNotas) * velocity * volumen * condicionalHiHat;

ingresoHiHat = ba.hz2midikey(frecuencia);

condicionalHiHat = ingresoHiHat==42;

sinteMono = hgroup("Percusion -8bit-", redo, hihatCerrado);

process = sinteMono,sinteMono;


