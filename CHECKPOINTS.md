# CHECKPOINTS — Evaluación del estado final

> En sistemas multi-agente no se evalúa el camino, se evalúa el destino.
> Estos son los checkpoints objetivos que un juez (humano o IA) puede usar
> para decidir si el proyecto está sano.

## C1 — El arnés está completo

- [ ] Existen los 4 archivos base: `AGENTS.md`, `init.sh`, `feature_list.json`,
      `progress/current.md`.
- [ ] Existen los 7 docs: `docs/architecture.md`, `docs/conventions.md`,
      `docs/verification.md`, `docs/workflow.md`, `docs/gherkin.md`,
      `docs/tdd.md`, `docs/mutation-testing.md`.
- [ ] `./init.sh` termina con exit code 0.

## C2 — El estado es coherente

- [ ] Como mucho una feature en `in_progress` en `feature_list.json`.
- [ ] Toda feature `done` tiene tests asociados que pasan.
- [ ] `progress/current.md` está vacío o describe la sesión activa
      (no contiene basura de sesiones anteriores).

## C3 — El código respeta la arquitectura

- [ ] `src/` solo contiene los módulos previstos en `docs/architecture.md`.
- [ ] No hay dependencias externas en `requirements.txt` (debe estar vacío
      o no existir).
- [ ] No hay `print()` sueltos para debug, ni TODOs sin contexto.

## C4 — La verificación es real

- [ ] `tests/` tiene al menos un test por módulo de `src/`.
- [ ] Los tests usan `tempfile.TemporaryDirectory()`, no mocks de fs.
- [ ] `python3 -m unittest discover -s tests -v` muestra > 0 tests
      y todos verdes.

## C5 — La sesión se cerró bien

- [ ] No hay archivos sin trackear sospechosos (`*.tmp`, `__pycache__`
      fuera del `.gitignore`).
- [ ] `progress/history.md` tiene una entrada por la última sesión.
- [ ] La última feature trabajada está reflejada en su estado correcto.

## C6 — Contrato Gherkin (BDD)

- [ ] Toda feature con `"sdd": true` en estado `spec_ready`, `in_progress`
      o `done` tiene su `features/<name>.feature` y una sección en
      `project-spec.md`.
- [ ] El `.feature` usa Gherkin con escenarios tagueados `@s1`, `@s2`, …
      y cada `Then` afirma algo medible (ver `docs/gherkin.md`).
- [ ] Cada escenario `@s` está cubierto por al menos un test concreto en
      `tests/` (mapa `@s → test` en `progress/tdd_<name>.md`).
- [ ] No hay código de producción que ningún test rojo haya pedido
      (disciplina TDD, ver `docs/tdd.md`).

## C7 — Prueba de mutación

- [ ] La feature `done` superó la prueba de mutación
      (`python3 tools/mutate.py src/<archivo>.py`) con la puntuación por
      encima del umbral de `docs/mutation-testing.md`.
- [ ] Cualquier mutante sobreviviente queda documentado en
      `progress/mutation_<name>.md` (matado con un test nuevo, o
      justificado como equivalente).

---

**Cómo usar este archivo:** el agente `judge` (`.claude/agents/judge.md`)
recorre C1-C6 y el `mutation_tester` valida C7. Se rechaza el cierre de
sesión si quedan boxes vacíos.
