<script>
export async function recupererQuestionnaires() {
  const response = await fetch('http://127.0.0.1:5000/questionnaires');
  const data = await response.json();
  return data;
}
</script>