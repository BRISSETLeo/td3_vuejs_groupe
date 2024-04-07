<script>
export async function recupererQuestionnaires() {
  try {
    const response = await fetch('http://127.0.0.1:5000/questionnaires');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Erreur lors de la récupération des questionnaires:", error);
    throw error;
  }
}

export async function creerQuestionnaire(formData) {
  try {
    const response = await fetch('http://127.0.0.1:5000/questionnaires', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(formData)
    });
  } catch (error) {
    console.error("Erreur lors de la création du questionnaire:", error);
    throw error;
  }
}

export async function supprimerQuestionnaire(id) {
  try {
    const response = await fetch(`http://127.0.0.1:5000/questionnaires/${id}`, {
      method: 'DELETE'
    });
  } catch (error) {
    console.error("Erreur lors de la suppression du questionnaire:", error);
    throw error;
  }
}

export async function recupererQuestions(id) {
  try {
    const response = await fetch(`http://127.0.0.1:5000/questionnaires/${id}/questions`);
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Erreur lors de la récupération des questions:", error);
    throw error;
  }
}

export async function creerQuestion(id, formData) {
  try {
    const response = await fetch(`http://127.0.0.1:5000/questionnaires/${id}/questions`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(formData)
    });
  } catch (error) {
    console.error("Erreur lors de la création de la question:", error);
    throw error;
  }
}

export async function modifierQuestion(id, formData) {
  try {
    const response = await fetch(`http://127.0.0.1:5000/questions/${id}`, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(formData)
    });
  } catch (error) {
    console.error("Erreur lors de la modification de la question:", error);
    throw error;
  }
}

export async function supprimerQuestion(id, id_question) {
  try {
    const response = await fetch(`http://127.0.0.1:5000/questionnaires/${id}/questions/${id_question}`, {
      method: 'DELETE'
    });
  } catch (error) {
    console.error("Erreur lors de la suppression de la question:", error);
    throw error;
  }
}
</script>
