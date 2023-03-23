<script>
import { ref } from 'vue';

console.log("i'm alive");

class Point {
  constructor(x, y, z) {
    this.x = x;
    this.y = y;
    this.z = z;
  }

  toString() {
    return `(${this.x}, ${this.y}, ${this.z})`;
  }
}

class Vecteur {
  constructor(point1, point2) {
    this.x = point2.x - point1.x;
    this.y = point2.y - point1.y;
    this.z = point2.z - point1.z;
  }

  produitVectoriel(v) {
    const x = this.y * v.z - this.z * v.y;
    const y = this.z * v.x - this.x * v.z;
    const z = this.x * v.y - this.y * v.x;
    return new Vecteur(new Point(0, 0, 0), new Point(x, y, z));
  }

  produitScalaire(vecteur) {
    return this.x * vecteur.x + this.y * vecteur.y + this.z * vecteur.z;
  }

  norm() {
    return Math.sqrt(this.x * this.x + this.y * this.y + this.z * this.z);
  }

  toString() {
    return `(${this.x}, ${this.y}, ${this.z})`;
  }
}

class Sphere {
  constructor(center, rayon) {
    this.center = center;
    this.rayon = rayon;
  }

  toString() {
    return `Sphere de centre ${this.center} et de rayon ${this.rayon}`;
  }
}

function distancePointSegment(A, B, C) {
  const v1 = new Vecteur(A, B);
  const v2 = new Vecteur(A, C);
  const produit = v2.produitVectoriel(v1);
  const norme = produit.norm();
  if (norme === 0) {
    // Les points B et C sont alignés, on renvoie la distance entre C et A ou B
    //return Math.min(distancePoints(A, C), distancePoints(B, C));
    const kac = v1.produitScalaire(v2);
    const kab = v1.produitScalaire(v1);

    if (kac < 0) {
      return kac.norm(); //for the moment we don't know how to behave if the object is located behind us
    }
    if (kac > kab) {
      return new Vecteur(B, C).norm();
    } //l'objet est derrière
    // (ne devrais pas pouvoir déclencher de distance <= sphere.rayon plus tard dans le code sinon erreur de logique)
    if (kac == 0 || kac == kab) {
      return -1;
    } //error in maths (shouldn't be able to trigger)

    if (kac > 0 && kac < kab) {
      return 0; // la voie est obstruée par un objet
    }
  }
  const normeAB = v1.norm();
  const distance = norme / normeAB;
  return distance;
}

function segmentPasseParSphere(A, B, sphere) {
  const distance = distancePointSegment(A, B, sphere.center);
  return distance <= sphere.rayon;
}

function spheresAccessibles(point, spheres) {
  const accessibles = [];
  for (const sphere of spheres) {
    let visible = true;
    for (const autreSphere of spheres) {
      if (sphere !== autreSphere) {
        const droitePasse = segmentPasseParSphere(
          point,
          sphere.center,
          autreSphere
        );
        if (droitePasse) {
          visible = false;
          break;
        }
      }
    }
    if (visible) {
      accessibles.push(sphere);
    }
  }
  return accessibles;
}

// Exemple d'utilisation
const A = new Point(0, 0, 0);
const B = new Point(1, 0, 0);
const C = new Point(0, 1, 0);
const distance = distancePointSegment(A, B, C);
console.log(distance); // affiche 1

const point = new Point(0, 0, 0);
const spheres = [
  new Sphere(new Point(1, 0, 0), 0.5),
  new Sphere(new Point(1, 1, 0), 0.5),
  new Sphere(new Point(0, 1, 0), 0.5),
  new Sphere(new Point(2, 0, 0), 0.5),
  new Sphere(new Point(2, 2, 0), 0.5),
];
const accessibles = spheresAccessibles(point, spheres);
console.log(accessibles); // affiche les sphères accessibles
</script>

<template></template>

<style></style>
