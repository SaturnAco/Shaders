# Shaders

Este proyecto consiste en un shader fragmentado animado inspirado en una protogema del juego genshin impact. La composición presenta un fondo azul nocturno con pequeñas estrellas blancas que parpadean. En el centro aparece un cristal de cuatro puntas que gira continuamente y cambia de tamaño. El cristal combina tonos azul hielo y rosa perlado, con un efecto luminoso en sus bordes.

> Autoría: Houyame Liazidi Daoudi · Asignatura: Informática Gráfica

## Vídeo de ejecución
https://youtube.com/shorts/Nxu4TTEp2iY?feature=share

## Versión legible
```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform float u_time;

void main() {

    // Coordenadas centradas y normalizadas.
    vec2 p = (gl_FragCoord.xy - 0.5 * u_resolution)
             / u_resolution.y;

    // Fondo azul nocturno.
    vec3 c = vec3(0.008, 0.018, 0.075);
    c += vec3(0.025, 0.045, 0.12)
         * exp(-2.0 * length(p));

    // Estrellas pequeñas que aparecen y desaparecen.
    vec2 q = fract(gl_FragCoord.xy / 42.0) - 0.564;
    c += vec3(0.8, 0.9, 1.0)
         * exp(-dot(q, q) * 4000.0)
         * abs(sin(u_time + gl_FragCoord.x));

    // Ángulo de la protogema: permite hacerla girar.
    float a = atan(p.y, p.x) - u_time;
    float r = length(p);

    // Tamaño variable: la protogema respira.
    float size = 0.17 * (1.0 + -0.9 * sin(2.0 * u_time));

    // Contorno de cristal de cuatro puntas.
    float d = r - size * (0.8 + 0.2 * cos(4.0 * a));

    // Resplandor difuminado del cristal.
    float glow = exp(-abs(d) * 80.0);

    // Degradado entre azul hielo y rosa perlado.
    vec3 crystal = mix(
        vec3(0.3, 0.7, 1.0),
        vec3(1.0, 0.6, 0.8),
        abs(sin(4.0 * a))
    );

    // Aplicación del brillo.
    c += crystal * glow;

    gl_FragColor = vec4(c, 1.0);
}

```
## Versión commpacta (509 bytes)
```glsl
precision mediump float;uniform vec2 u_resolution;uniform float u_time;void main(){vec2 p=(gl_FragCoord.xy-.5*u_resolution)/u_resolution.y;vec3 c=vec3(.008,.018,.075)+vec3(.025,.045,.12)*exp(-2.*length(p));vec2 q=fract(gl_FragCoord.xy/42.)-.564;c+=vec3(.8,.9,1.)*exp(-dot(q,q)*4000.)*abs(sin(u_time+gl_FragCoord.x));float a=atan(p.y,p.x)-u_time,r=length(p),s=.17*(1.-.9*sin(2.*u_time)),d=r-s*(.8+.2*cos(4.*a)),g=exp(-abs(d)*80.);c+=mix(vec3(.3,.7,1.),vec3(1.,.6,.8),abs(sin(4.*a)))*g;gl_FragColor=vec4(c,1.);}
```

## Fuentes
- Prácticas 9 y 10 de la asignatura (código de referencia).
- https://thebookofshaders.com/
