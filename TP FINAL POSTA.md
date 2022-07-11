//Percusión -8bits-
import("stdfaust.lib");


 frecuencia = nentry("freq", 59, 59, 300, 1);
 velocity = nentry("gain", 1, 0, 1, 0.01);
 gateDeNotas = button("gate");

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

hihatCerrado = no.noise*  en.adsr(attackHiHat , decayHiHat , sustainHiHat , releaseHiHat , gateDeNotas) * velocity * volumen/4 * condicionalHiHat;

ingresoHiHat = ba.hz2midikey(frecuencia);

condicionalHiHat = ingresoHiHat==42;

// Bombo

//envolvente 1
attackBombo = 0;
decayBombo = 1950;
sustainBombo = 0;
releaseBombo = 0.001;


//envolvente 2

attackBombo2 = 0.004;
decayBombo2 = 0.216;
sustainBombo2 = 0;
releaseBombo2 = 0.050;


bombo = os.osc(frecuencia)* en.adsr(attackBombo, decayBombo, sustainBombo, releaseBombo, gateDeNotas) * velocity * volumen* en.adsr(attackBombo2, decayBombo2, sustainBombo2, releaseBombo2, gateDeNotas)* condicionalBombo;


ingresoBombo = ba.hz2midikey(frecuencia);

condicionalBombo = ingresoBombo==38;

sinteMono = hgroup("Percusion -8bit-", redo, hihatCerrado, bombo);

percusion8bit = sinteMono:>_;

volumen = vslider("master", 0.5, 0, 1, 0.01);

process = percusion8bit,percusion8bit;

