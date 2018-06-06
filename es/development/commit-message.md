---
title: "Formato de un Git Commit"
description: "Recomendación sobre cómo generar el mensaje al realizar un Commit Git"
---

## Formato de un Commit
Si usas un software con la funcionalidad de "smart commit" (*como JIRA*), te recomendamos que declare el id de la historia en curso asociada con el commit.

Las recomendaciónes de como estructurar un mensaje git son:

### Id de la historia en curso (como el identificador de JIRA)
Cuando una historia está identificada y se encuentra presente, es fácil obtener la información sobre el objetivo que rige tu commit actual, por lo que recomendamos encarecidamente que lo añadas en el mensaje git cuando lo realices.

### Categoría del Commit
Es útil categorizar un mensaje porque permite a los equipos que revisan el cambio, obtener la "imagen general" de los cambios realizados, y reducir los archivos revisados durante la revisión.

Nosotros tenemos 3 categorías que se pueden usar para categorizar:
- **feature**: Una nueva funcionalidad asociada al proyecto en curso.
- **fix**: Una corrección sobre una funcionalidad o modificación de una incidencia ya reportada o error.
- **upgrade**: Una mejora sobre una funcionalidad que ya se encuentra y que añade nuevas funcionalidades asociadas.

> Recomendamos encarecidamente que el idioma para generar el mensaje sea en ingles ^^.

### Estructura

> **[CARD_ID|no-history]** (**[category]**): **[inline description]**

#### Ejemplo:
> JIRA-1 (feature): add deployer api server && up test > 50%
