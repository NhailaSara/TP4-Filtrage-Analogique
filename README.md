# TP4-Filtrage-Analogique


#Objectifs
-Appliquer un filtre réel pour supprimer les composantes indésirables d’un signal.
-Améliorer la qualité de filtrage en augmentant l’ordre du filtre.

# Filtrage et diagramme de Bode
1)Définir le signal x(t) sur t = [0 5] avec Te = 0,0001 s.

![1](https://user-images.githubusercontent.com/121027279/215218453-3612612c-19eb-4b83-b18b-5e2ce216c133.png)

2)Tracer le signal x(t) et sa transformé de Fourrier
![2](https://user-images.githubusercontent.com/121027279/215218543-a31d0211-985c-43c2-be4b-0f2e34e98dc7.png)
on remarque que le premier signal n'est pas précis
![3](https://user-images.githubusercontent.com/121027279/215218674-f366e0d0-78a7-4c61-88d4-24bce8cfb82e.png)

On a H(f) = (K.j.w/wc) / (1 + j.w/wc)

1)Tracer le module de la fonction H(f) avec K=1 et wc = 50 rad/s

<img width="467" alt="4" src="https://user-images.githubusercontent.com/121027279/215218834-3f4af39c-e727-4df9-b596-7311317f002c.png">

      % frequence wc=50
      H = (K*1i*w/wc)./(1+1i*w/wc);
      semilogx(f,abs(H));
      
  2)Tracer 20.log(|H(f)|) pour différentes pulsations de coupure wc
  
  <img width="481" alt="5" src="https://user-images.githubusercontent.com/121027279/215218976-10d16b95-324f-494a-a254-a193962aabe7.png">

      G = 20*log(abs(H));
      semilogx(f,G);
      
  3)Choisissez différentes fréquences de coupure et appliquez ce filtrage dans l'espace des fréquences.
  
  ![6](https://user-images.githubusercontent.com/121027279/215219109-0f618753-f68f-4b2e-ac9c-7528ca11d611.png)


     subplot(2,2,1);
    
        semilogx(f,abs(H),f,abs(H1),f,abs(H2),f,abs(H3));
        legend("spectre du signal avec wc=50","spectre du signal avec wc1=2*pi*500"," spectre du signal avec wc2=2*pi*1000","spectre du signal avec                wc3=2*pi*50");  
        grid on
        xlabel("f");
        ylabel("|H(jw)|");
      subplot(2,2,2);
        semilogx(f,G,f,G1,f,G2,f,G3);
        legend("Courbe de Gain  wc=50","Courbe de Gain wc1=2*pi*500","Courbe de Gain wc2=2*pi*1000","Courbe de Gain wc3=2*pi*50");  
        grid on
        xlabel("f");
        ylabel("20*log(|H(jw)|)");
    
        subplot(2,2,3);
    
        semilogx(f,Ang,f,Ang1,f,Ang2,f,Ang3);
        grid on
        legend("Courbe de dephasage wc=50","Courbe de dephasage wc1=2*pi*500","Courbe de dephasage wc2=2*pi*1000","Courbe de dephasage wc3=2*pi*50"); 
        xlabel("f");
        ylabel("angle(H(jw))");
        
        4)Choisissez wc qui vous semble optimal. Le filtre est-il bien choisi ? Pourquoi?
        
        On a appliqué sur le signal la transmittance complexe de fréquence 500 on peut remarqué une attenuation sur tout le signal mais elle est plus importante dans les basses fréquence.
        5)Observez le signal y(t) obtenu
        <img width="1050" alt="7" src="https://user-images.githubusercontent.com/121027279/215219288-2f5ec666-2b3f-4fee-9294-721022d1a54a.png">
#Dé-bruitage d'un signal sonore
1)Proposer une méthode pour supprimer ce bruit sur le signal.
filtrage
2) Mettez-la en oeuvre
<img width="1209" alt="8" src="https://user-images.githubusercontent.com/121027279/215219434-7dc542ae-a81f-4c0b-8038-b297f6ba4c5a.png">


        fc = 5000;
         K =1;

         H = K./(1+1i*(f/fc).^100);
         Hpass=[H(1:floor(N/2)),flip(H(1:floor(N/2)))];


        y_filtre = spectre_music(1:end-1).*Hpass;
        sig_filtred= ifft(y_filtre,"symmetric");
        plot(fshift(1:end-1),fftshift(abs(fft(sig_filtred))))
        legend("spectre du signal aprés filtrage"); 
        xlabel("f");
        ylabel("A");
        
        
  3) Quelles remarques pouvez-vous faire notamment sur la sonorité du signal final.

le filtre analogique ne fait que diminué le bruit et comme on a vu présedement lors de l'attenuation du bruit dans I autre signal on arrive pas a filtrer le signal avec une transmittance complexe d'ordre 1 sans affecter le signal. Donc un filtre pass bas de premier ordre ne sera pas efficace pour eliminé le bruit
4) Améliorer la qualité de filtrage en augmentant l’ordre du filtre.
<img width="561" alt="9" src="https://user-images.githubusercontent.com/121027279/215219540-39ea0ad0-8457-412d-a453-bf0956bd81e7.png">

on utilise un filtre butterworth
        
