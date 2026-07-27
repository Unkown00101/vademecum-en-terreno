/* Vademécum APS Coquimbo — aplicación offline.
   Los datos oficiales provienen de la Resolución N.º 2504243202 (SEREMI Coquimbo, 11/09/2025).
   Esta aplicación ordena y consulta esos datos: no los reemplaza ni los interpreta clínicamente. */
(function () {
'use strict';

var D = window.VADE;
if (!D) { document.body.innerHTML = '<p style="padding:20px">No se pudo cargar data.js.</p>'; return; }

/* ───────────────────────── utilidades ───────────────────────── */

function esc(s) {
  return String(s == null ? '' : s)
    .replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;').replace(/'/g, '&#39;');
}
function norm(s) {
  s = String(s == null ? '' : s).toLowerCase();
  try { s = s.normalize('NFD').replace(/[\u0300-\u036f]/g, ''); } catch (e) {}
  return s;
}
function slug(s) {
  return norm(s).replace(/[^a-z0-9]+/g, '-').replace(/^-|-$/g, '');
}
var Guardar = {
  leer: function (k, alt) {
    try { var v = localStorage.getItem('vade.' + k); return v == null ? alt : JSON.parse(v); }
    catch (e) { return (Guardar._m && Guardar._m[k] !== undefined) ? Guardar._m[k] : alt; }
  },
  escribir: function (k, v) {
    try { localStorage.setItem('vade.' + k, JSON.stringify(v)); }
    catch (e) { Guardar._m = Guardar._m || {}; Guardar._m[k] = v; }
  },
  borrar: function (k) {
    try { localStorage.removeItem('vade.' + k); } catch (e) { if (Guardar._m) delete Guardar._m[k]; }
  }
};

/* Separa los llamados a nota al pie pegados al texto: "TBC3" → "TBC" + nota 3 */
function separarNotas(txt) {
  var refs = [];
  var limpio = String(txt || '').replace(
    /([A-Za-zÁÉÍÓÚÑáéíóúñ\)\.])(\d+(?:,\d+)*)/g,
    function (m, a, d) { d.split(',').forEach(function (n) { if (refs.indexOf(n) < 0) refs.push(n); }); return a; }
  );
  return { texto: limpio.trim(), notas: refs };
}

var ARS_CORTO = ['CESFAM / Centro APS', 'SAPU / SAR', 'Posta rural'];
var ARS_LARGO = ['CESFAM o centro de salud APS', 'dispositivo de urgencia (SAPU / SAR)', 'posta de salud rural'];

/* ───────────────────── índice de búsqueda ───────────────────── */

D.meds.forEach(function (m, i) {
  m.i = i;
  m.slug = slug(m.n);
  m.nn = norm(m.n);
  m.aliasn = (m.o || []).map(norm).join(' | ');
  var extra = [];
  m.p.forEach(function (r) { extra.push(r[0], r[1], r[2], r[3], r[10]); });
  m.hay = norm([m.n, (m.o || []).join(' '), m.cat, m.cls, m.sub, extra.join(' ')].join(' '));
  m.arsenales = {};
  m.p.forEach(function (r) { m.arsenales[r[5]] = true; });
  m.etiquetas = {};
  m.p.forEach(function (r) {
    (r[7] || '').split(';').forEach(function (t) { t = t.trim(); if (t) m.etiquetas[t] = true; });
  });
});
var porSlug = {};
D.meds.forEach(function (m) { porSlug[m.slug] = m; });

/* ─────────────── motor de disponibilidad por establecimiento ─────────────── */

/* Devuelve {nivel:0|1|2, titular, condiciones:[{cl,txt}], propias:[], otras:[]} */
function veredicto(m, est) {
  var propias = m.p.filter(function (r) { return r[5] === est; });
  var otras = m.p.filter(function (r) { return r[5] !== est; });
  var cond = [], nivel = 0;

  if (!propias.length) {
    var donde = {};
    otras.forEach(function (r) { donde[r[5]] = true; });
    var lista = Object.keys(donde).map(function (a) { return ARS_CORTO[+a]; }).join(' y ');
    return {
      nivel: 2, propias: propias, otras: otras,
      titular: 'No está en el arsenal de ' + ARS_CORTO[est].toLowerCase(),
      condiciones: [{
        cl: 'Dónde sí',
        txt: lista
          ? 'La resolución lo autoriza en <b>' + esc(lista) + '</b>. Para entregarlo aquí se requiere gestión con el nivel que corresponda.'
          : 'No figura en ninguno de los tres arsenales de esta resolución.'
      }]
    };
  }

  var niveles = propias.map(function (r) {
    var t = (r[7] || ''), n = 0;
    if (t.indexOf('Sólo hospitales') >= 0) n = 2;
    else if (t.indexOf('Continuidad') >= 0 || t.indexOf('Sin registro sanitario') >= 0 ||
             t.indexOf('Sólo SAR') >= 0 || t.indexOf('SAPU/SAR') >= 0 || (r[3] || '').trim()) n = 1;
    return n;
  });
  nivel = Math.min.apply(null, niveles);

  var e = m.etiquetas, tienePropia = function (t) {
    return propias.some(function (r) { return (r[7] || '').indexOf(t) >= 0; });
  };

  if (tienePropia('Sólo hospitales'))
    cond.push({ cl: 'Hospital', txt: 'La resolución restringe la disponibilidad de este producto a <b>hospitales</b>. No se dispensa en APS.' });

  if (tienePropia('Continuidad'))
    cond.push({ cl: 'Continuidad', txt: 'Sólo para <b>continuidad de tratamiento crónico</b> con prescripción médica vigente. Su existencia en la posta depende de si hay pacientes en tratamiento.' });

  if (tienePropia('Sin registro sanitario'))
    cond.push({ cl: 'Registro', txt: 'Producto <b>sin registro sanitario vigente</b> o distribuido por CENABAST / otra vía autorizada. Verificar procedencia y trazabilidad del lote.' });

  if (tienePropia('Sólo SAR'))
    cond.push({ cl: 'Sólo SAR', txt: 'Uso restringido a <b>SAR</b>. No corresponde en SAPU ni en otros dispositivos.' });
  else if (tienePropia('SAPU/SAR'))
    cond.push({ cl: 'SAPU y SAR', txt: 'Uso restringido a <b>SAPU y SAR</b>.' });

  var programas = {};
  propias.forEach(function (r) {
    var g = separarNotas(r[3]).texto;
    if (g) programas[g] = true;
  });
  var progs = Object.keys(programas);
  if (progs.length)
    cond.push({ cl: 'Programa', txt: 'Entrega asociada a <b>' + esc(progs.join(' · ')) + '</b>. Verificar que la persona esté bajo control en el programa correspondiente.' });

  if (tienePropia('Carro de paro'))
    cond.push({ cl: 'Carro de paro', txt: 'Presentación consignada para <b>carro de paro</b>. Mantener sellado, con control de stock y de vencimiento.' });

  if (tienePropia('Alternativa terapéutica'))
    cond.push({ cl: 'Alternativa', txt: 'La resolución autoriza una <b>alternativa terapéutica</b> para este ítem (ver más abajo).' });

  var titular, mixto = nivel === 0 && niveles.some(function (n) { return n > 0; });
  if (nivel === 0) titular = mixto
    ? 'Disponible, según la presentación'
    : 'Disponible en ' + ARS_CORTO[est].toLowerCase();
  else if (nivel === 1) titular = 'Disponible con condiciones';
  else titular = 'No se dispensa en APS';

  if (mixto) cond.unshift({ cl: 'Ojo', txt: 'No todas las presentaciones tienen la misma condición. Revisá cuál corresponde antes de entregar.' });

  return { nivel: nivel, titular: titular, condiciones: cond, propias: propias, otras: otras };
}
var CLASE = ['si', 'cond', 'no'];

/* alternativas terapéuticas declaradas en observaciones */
function alternativas(r) {
  var o = r[4] || '';
  var m = o.split(/Alternativa\s+Terap[eé]utica\s*\d*\s*:/i);
  if (m.length < 2) return [];
  return m.slice(1).map(function (t) { return t.trim().replace(/\s+/g, ' '); })
    .filter(Boolean)
    .reduce(function (acc, t) { return acc.concat(t.split(/\s+y\/o\s+/i)); }, [])
    .map(function (t) { return t.trim().replace(/^\.|\.$/, '').trim(); })
    .filter(Boolean);
}

/* ─────────────────────────── estado ─────────────────────────── */

var S = {
  est: Guardar.leer('estab', 2),
  q: '',
  filtros: { estab: true, paro: false, alerta: false, ges: false },
  cat: null,
  ruta: 'buscar',
  med: null
};

var $ = function (s) { return document.querySelector(s); };
var selEstab = $('#selEstab');
selEstab.value = String(S.est);
selEstab.addEventListener('change', function () {
  S.est = +selEstab.value;
  Guardar.escribir('estab', S.est);
  pintarChips();
  pintar();
});

/* ─────────────────────────── buscador ─────────────────────────── */

function buscar() {
  var q = norm(S.q.trim());
  var res = D.meds.filter(function (m) {
    if (S.filtros.estab && !m.arsenales[S.est]) return false;
    if (S.filtros.paro && !m.etiquetas['Carro de paro']) return false;
    if (S.filtros.alerta && !(m.etiquetas['Sin registro sanitario'] || m.etiquetas['Sólo hospitales'])) return false;
    if (S.filtros.ges && !m.p.some(function (r) { return (r[3] || '').trim(); })) return false;
    if (S.cat && m.cat !== S.cat) return false;
    if (!q) return true;
    return m.hay.indexOf(q) >= 0;
  });
  if (q) {
    res.forEach(function (m) {
      m._s = m.nn.indexOf(q) === 0 ? 0 : (m.nn.indexOf(q) >= 0 ? 1 : (m.aliasn.indexOf(q) >= 0 ? 2 : 3));
    });
    res.sort(function (a, b) { return a._s - b._s || a.nn.localeCompare(b.nn); });
  }
  return res;
}

function filaHTML(m) {
  var v = veredicto(m, S.est);
  var formas = {};
  (v.propias.length ? v.propias : m.p).forEach(function (r) { formas[r[1]] = true; });
  var sub = Object.keys(formas).slice(0, 3).join(' · ');
  var ins = '';
  if (m.etiquetas['Carro de paro']) ins += '<span class="insignia paro">Carro de paro</span>';
  if (m.etiquetas['Sin registro sanitario']) ins += '<span class="insignia alerta">Sin registro</span>';
  return '<li><button class="fila ' + CLASE[v.nivel] + '" data-med="' + esc(m.slug) + '">' +
    '<span class="marca-estado"></span><span class="cuerpo">' +
    '<span class="nombre">' + esc(m.n) + ins + '</span>' +
    '<span class="sub">' + esc(m.cat) + (sub ? ' · ' + esc(sub) : '') + '</span>' +
    '<span class="cond-linea">' + esc(v.titular) +
    (v.nivel === 1 && v.condiciones.length ? ' — ' + esc(v.condiciones[0].cl.toLowerCase()) : '') +
    '</span></span></button></li>';
}

function pintarBuscar() {
  var cont = $('#v-buscar');
  var hayFiltro = S.q || S.cat || S.filtros.paro || S.filtros.alerta || S.filtros.ges || !S.filtros.estab;

  if (!hayFiltro) { cont.innerHTML = portada(); enlazarPortada(cont); return; }

  var res = buscar();
  var html = '<div class="meta-resultados"><span class="rotulo">' +
    res.length + (res.length === 1 ? ' medicamento' : ' medicamentos') + '</span>' +
    (S.cat ? '<button class="rotulo" id="quitarCat" style="color:var(--teal)">Quitar «' + esc(S.cat) + '» ✕</button>' : '') +
    '</div>';
  html += res.length
    ? '<ul class="lista">' + res.map(filaHTML).join('') + '</ul>'
    : '<div class="vacio"><strong>Sin coincidencias</strong>' +
      'Probá con el nombre genérico, la forma farmacéutica o el registro ISP. ' +
      'Si buscás algo que no está aquí, no forma parte del arsenal aprobado por la resolución.</div>';
  cont.innerHTML = html;
  var qc = cont.querySelector('#quitarCat');
  if (qc) qc.addEventListener('click', function () { S.cat = null; pintar(); });
}

function portada() {
  var enEst = D.meds.filter(function (m) { return m.arsenales[S.est]; });
  var pres = 0, cond = 0;
  enEst.forEach(function (m) {
    m.p.forEach(function (r) { if (r[5] === S.est) pres++; });
    if (veredicto(m, S.est).nivel > 0) cond++;
  });
  var cats = {};
  enEst.forEach(function (m) { cats[m.cat] = (cats[m.cat] || 0) + 1; });
  var orden = Object.keys(cats).sort(function (a, b) { return cats[b] - cats[a] || a.localeCompare(b); });

  var h = '<div id="ranuraInstalar"></div>';
  h += '<div class="tarjeta"><h2>Tu arsenal en ' + esc(ARS_CORTO[S.est].toLowerCase()) + '</h2>' +
    '<div class="resumen-estab">' +
    '<div><span class="cifra">' + enEst.length + '</span><span class="et">medicamentos<br>y productos</span></div>' +
    '<div><span class="cifra">' + pres + '</span><span class="et">presentaciones<br>autorizadas</span></div>' +
    '<div><span class="cifra" style="color:var(--cond)">' + cond + '</span><span class="et">con alguna<br>condición</span></div>' +
    '</div></div>';
  h += '<div class="tarjeta"><h2>Por categoría terapéutica</h2><div class="rejilla-cat">' +
    orden.map(function (c) {
      return '<button class="btn-cat" data-cat="' + esc(c) + '"><span>' + esc(c) + '</span><span class="n">' + cats[c] + '</span></button>';
    }).join('') + '</div></div>';
  return h;
}

function enlazarPortada(cont) {
  cont.querySelectorAll('[data-cat]').forEach(function (b) {
    b.addEventListener('click', function () { S.cat = b.getAttribute('data-cat'); pintar(); });
  });
  montarInstalar();
}

/* ─────────────────────────── ficha ─────────────────────────── */

function pintarMed() {
  var m = S.med, cont = $('#v-med');
  if (!m) { cont.innerHTML = ''; return; }
  var v = veredicto(m, S.est);

  var h = '<div class="ficha-cabecera">' +
    '<button class="btn-volver" id="volver">' +
    '<svg class="icono" viewBox="0 0 24 24" style="width:15px;height:15px"><path d="m14 6-6 6 6 6"/></svg>Volver</button>' +
    '<h2>' + esc(m.n) + '</h2>' +
    (m.o && m.o.length ? '<div class="alias">En la resolución también aparece como: ' + esc(m.o.join(' · ')) + '</div>' : '') +
    '<div class="taxonomia">' +
    ['<span class="tax">' + esc(m.cat) + '</span>',
     m.cls ? '<span class="tax">' + esc(m.cls) + '</span>' : '',
     m.tp && m.tp !== 'Medicamento' ? '<span class="tax">' + esc(m.tp) + '</span>' : ''].join('') +
    '</div></div>';

  /* banda de veredicto */
  h += '<div class="veredicto ' + CLASE[v.nivel] + '"><div class="encabezado">' +
    '<div class="lugar">' + esc(ARS_CORTO[S.est]) + '</div>' +
    '<div class="titular">' + esc(v.titular) + '</div></div>';
  if (v.condiciones.length) {
    h += '<ul class="condiciones">' + v.condiciones.map(function (c) {
      return '<li><span class="cl">' + esc(c.cl) + '</span><span>' + c.txt + '</span></li>';
    }).join('') + '</ul>';
  }
  h += '</div>';

  /* presentaciones */
  var grupos = [[], [], []];
  m.p.forEach(function (r) { grupos[r[5]].push(r); });
  var orden = [S.est, 0, 1, 2].filter(function (a, i, arr) { return arr.indexOf(a) === i; });

  h += '<div class="tarjeta"><h2>Presentaciones autorizadas</h2>';
  orden.forEach(function (a) {
    if (!grupos[a].length) return;
    h += '<div class="grupo-arsenal"><span class="rotulo">' + esc(D.arsenales[a]) +
      (a === S.est ? ' · tu establecimiento' : '') + '</span>';
    grupos[a].forEach(function (r) {
      var ges = separarNotas(r[3]);
      var notas = ges.notas.concat(r[8] ? [r[8]] : []);
      var alts = alternativas(r);
      var obsCruda = (r[4] || '').replace(/Alternativa\s+Terap[eé]utica\s*\d*\s*:.*$/i, '').trim();
      var obsSep = separarNotas(obsCruda);
      var obsLimpia = obsSep.texto;
      obsSep.notas.forEach(function (n) { if (notas.indexOf(n) < 0) notas.push(n); });
      h += '<div class="pres' + (a === S.est ? ' propia' : '') + '">' +
        '<div class="linea1"><span class="forma">' + esc(r[1]) + '</span>' +
        '<span class="conc mono">' + esc(r[2]) + '</span></div>' +
        '<div class="linea2">' +
        '<span class="isp mono">ISP ' + esc(r[0] || '—') + '</span>' +
        (ges.texto ? '<span class="etq prog">' + esc(ges.texto) + '</span>' : '') +
        ((r[7] || '').indexOf('Continuidad') >= 0 ? '<span class="etq cont">Continuidad de tratamiento</span>' : '') +
        ((r[7] || '').indexOf('Carro de paro') >= 0 ? '<span class="etq paro">Carro de paro</span>' : '') +
        ((r[7] || '').indexOf('Móvil SAPU') >= 0 ? '<span class="etq">Móvil SAPU</span>' : '') +
        ((r[7] || '').indexOf('Sin registro sanitario') >= 0 ? '<span class="etq sr">Sin registro vigente</span>' : '') +
        '<span class="etq">' + esc(r[6]) + '</span>' +
        '</div>';
      if (obsLimpia) h += '<div class="obs"><b>Observación oficial:</b> ' + esc(obsLimpia) + '</div>';
      if (alts.length) {
        h += '<div class="obs"><b>Si no hay stock, la resolución autoriza:</b></div>';
        alts.forEach(function (t) {
          h += '<div class="alt"><span class="flecha">→</span><span>' + esc(t) + '</span></div>';
        });
      }
      notas.forEach(function (n) {
        var txt = (D.notas[D.arsenales[a]] || {})[n];
        if (txt) h += '<div class="obs"><b>Nota ' + esc(n) + ':</b> ' + esc(txt) + '</div>';
      });
      h += '</div>';
    });
    h += '</div>';
  });
  h += '</div>';

  /* resumen orientativo, claramente separado */
  var C = D.campos_ficha, F = m.f, T = D.T;
  var etiquetas = {
    indicaciones_principales: 'Para qué se usa',
    mecanismo_resumido: 'Cómo actúa',
    reacciones_frecuentes: 'Reacciones frecuentes',
    alertas_graves: 'Alertas graves',
    contraindicaciones_precauciones: 'Contraindicaciones y precauciones',
    interacciones_clave: 'Interacciones a revisar',
    monitorizacion: 'Qué monitorizar',
    ajuste_renal_hepatico: 'Ajuste renal o hepático',
    embarazo_lactancia: 'Embarazo y lactancia',
    educacion_paciente: 'Educación a la persona',
    rol_enfermeria: 'Rol de enfermería',
    checklist_previa: 'Verificación previa a la administración'
  };
  h += '<details class="orientativo"><summary>Resumen clínico orientativo' +
    '<span class="rotulo" style="color:var(--teal)">Abrir</span></summary>' +
    '<div class="aviso"><b>Este bloque no proviene de la resolución.</b> Es un resumen redactado a nivel de ' +
    'grupo farmacológico, no específico de este principio activo, y no incluye dosis. ' +
    'Antes de administrar, verificá la ficha técnica vigente del producto, la prescripción y el protocolo local.</div>';
  if (m.key) h += '<div class="campo"><span class="rotulo">Consideración clave</span><p>' + esc(m.key) + '</p></div>';
  Object.keys(etiquetas).forEach(function (k) {
    var idx = C.indexOf(k); if (idx < 0) return;
    var val = F[idx] >= 0 ? T[F[idx]] : '';
    if (!val) return;
    h += '<div class="campo"><span class="rotulo">' + esc(etiquetas[k]) + '</span><p>' + esc(val) + '</p></div>';
  });
  h += '<div class="campo"><span class="rotulo">Dosis</span><p>La resolución autoriza presentaciones y ' +
    'concentraciones, pero <b>no define esquemas de dosificación</b>. La dosis se toma de la prescripción, ' +
    'del protocolo local vigente y de la ficha técnica, considerando edad, peso, indicación, embarazo, ' +
    'función renal o hepática e interacciones.</p></div>';
  h += '</details>';

  var pags = {};
  m.p.forEach(function (r) { if (r[9]) pags[r[9]] = true; });
  h += '<div class="pie-fuente">Datos oficiales: Resolución N.º 2504243202, SEREMI de Salud Región de Coquimbo, ' +
    '11/09/2025 — página ' + Object.keys(pags).sort(function (a, b) { return a - b; }).join(', ') + ' del PDF. ' +
    'Validación del documento en <code>seremienlinea.minsal.cl</code>, trámite <code>2504243202</code>.</div>';

  cont.innerHTML = h;
  cont.querySelector('#volver').addEventListener('click', function () {
    ir('buscar');
  });
  window.scrollTo(0, 0);
}

/* ─────────────────────── carro de paro ─────────────────────── */

function itemsParo() {
  var out = [];
  D.meds.forEach(function (m) {
    m.p.forEach(function (r) {
      if ((r[7] || '').indexOf('Carro de paro') >= 0) {
        out.push({ id: (r[0] || m.slug) + '|' + slug(r[1]) + '|' + slug(r[2]), med: m, r: r });
      }
    });
  });
  out.sort(function (a, b) { return a.med.nn.localeCompare(b.med.nn); });
  return out;
}

function estadoVenc(v) {
  if (!v) return '';
  var hoy = new Date(), y = +v.slice(0, 4), mo = +v.slice(5, 7);
  var fin = new Date(y, mo, 0);
  var dias = Math.round((fin - hoy) / 86400000);
  if (dias < 0) return 'vencido';
  if (dias <= 90) return 'pronto';
  return 'ok';
}

function pintarCarro() {
  var items = itemsParo();
  var reg = Guardar.leer('paro', {});
  var hechos = items.filter(function (it) { return reg[it.id] && reg[it.id].ok; }).length;
  var vencidos = items.filter(function (it) { return reg[it.id] && estadoVenc(reg[it.id].v) === 'vencido'; }).length;
  var prontos = items.filter(function (it) { return reg[it.id] && estadoVenc(reg[it.id].v) === 'pronto'; }).length;

  var h = '<div class="tarjeta"><h2>Revisión del carro de paro</h2>' +
    '<p style="margin:0;font-size:13.5px;color:var(--tinta-2)">' + items.length +
    ' presentaciones consignadas para carro de paro en la resolución. ' +
    'Marcá lo verificado, anotá lote y vencimiento, y generá el acta al terminar. ' +
    'Todo queda guardado en este dispositivo, no se envía a ninguna parte.</p>' +
    '<div class="progreso"><i style="width:' + Math.round(hechos / items.length * 100) + '%"></i></div>' +
    '<div style="display:flex;gap:14px;margin-top:8px;flex-wrap:wrap;font-size:12.5px">' +
    '<span><b class="mono">' + hechos + '/' + items.length + '</b> verificados</span>' +
    (vencidos ? '<span style="color:var(--no)"><b class="mono">' + vencidos + '</b> vencidos</span>' : '') +
    (prontos ? '<span style="color:var(--cond)"><b class="mono">' + prontos + '</b> vencen en 3 meses o menos</span>' : '') +
    '</div>' +
    '<div class="item-paro" style="box-shadow:none;padding:0;margin:12px 0 0">' +
    '<div class="campos" style="grid-template-columns:1fr 1fr">' +
    '<label><span>Establecimiento</span><input type="text" id="fLugar" placeholder="Posta de Salud Rural…" value="' +
      esc(Guardar.leer('lugar', '') || '') + '"></label>' +
    '<label><span>Responsable de la revisión</span><input type="text" id="fResp" placeholder="Nombre y cargo" value="' +
      esc(Guardar.leer('responsable', '') || '') + '"></label>' +
    '</div></div>' +
    '<div class="acciones no-imprimir">' +
    '<button class="btn" id="acta">Generar acta</button>' +
    '<button class="btn secundario" id="marcarTodo">Marcar todo</button>' +
    '<button class="btn peligro" id="reiniciar">Reiniciar revisión</button>' +
    '</div></div>';

  h += items.map(function (it) {
    var e = reg[it.id] || {};
    var ev = estadoVenc(e.v);
    return '<div class="item-paro' + (e.ok ? ' hecho' : '') + (ev === 'vencido' ? ' vence' : '') + '" data-id="' + esc(it.id) + '">' +
      '<div class="fila1">' +
      '<input type="checkbox" ' + (e.ok ? 'checked' : '') + ' aria-label="Verificado">' +
      '<div class="txt"><div class="nom">' + esc(it.med.n) + '</div>' +
      '<div class="det">' + esc(it.r[1]) + ' · ' + esc(it.r[2]) + (it.r[0] ? ' · ISP ' + esc(it.r[0]) : '') + '</div>' +
      ((it.r[7] || '').indexOf('Móvil SAPU') >= 0 ? '<div style="margin-top:5px"><span class="etq">También móvil SAPU</span></div>' : '') +
      '</div></div>' +
      '<div class="campos"><label><span>Lote</span><input type="text" data-c="l" value="' + esc(e.l || '') + '"></label>' +
      '<label><span>Vence</span><input type="month" data-c="v" value="' + esc(e.v || '') + '"></label>' +
      '<label><span>Cant.</span><input type="number" data-c="c" min="0" value="' + esc(e.c == null ? '' : e.c) + '"></label></div>' +
      '</div>';
  }).join('');

  var cont = $('#v-carro');
  cont.innerHTML = h;

  if (!cont.dataset.enlazado) {
    cont.dataset.enlazado = '1';
    cont.addEventListener('input', manejarParo);
    cont.addEventListener('change', manejarParo);
    cont.addEventListener('click', function (ev) {
      if (!ev.target || !ev.target.closest) return;
      var b = ev.target.closest('button'); if (!b) return;
      if (b.id === 'marcarTodo') {
        var r = Guardar.leer('paro', {});
        itemsParo().forEach(function (it) { r[it.id] = r[it.id] || {}; r[it.id].ok = true; });
        Guardar.escribir('paro', r); pintarCarro();
      } else if (b.id === 'reiniciar') {
        if (confirm('Se borran todas las marcas, lotes y vencimientos guardados en este dispositivo. ¿Continuar?')) {
          Guardar.borrar('paro'); pintarCarro();
        }
      } else if (b.id === 'acta') generarActa();
    });
  }
}

function manejarParo(ev) {
  if (!ev.target || !ev.target.closest) return;
  if (ev.target.id === 'fLugar') { Guardar.escribir('lugar', ev.target.value); return; }
  if (ev.target.id === 'fResp') { Guardar.escribir('responsable', ev.target.value); return; }
  var caja = ev.target.closest('.item-paro[data-id]'); if (!caja) return;
  var id = caja.getAttribute('data-id');
  var reg = Guardar.leer('paro', {}); reg[id] = reg[id] || {};
  if (ev.target.type === 'checkbox') { reg[id].ok = ev.target.checked; caja.classList.toggle('hecho', ev.target.checked); }
  else {
    var c = ev.target.getAttribute('data-c');
    if (c) reg[id][c] = ev.target.value;
    if (c === 'v') caja.classList.toggle('vence', estadoVenc(ev.target.value) === 'vencido');
  }
  Guardar.escribir('paro', reg);
}

function generarActa() {
  var items = itemsParo(), reg = Guardar.leer('paro', {});
  var lugar = (document.getElementById('fLugar') || {}).value || '—';
  var resp = (document.getElementById('fResp') || {}).value || '—';

  var f = new Date();
  var fecha = f.toLocaleDateString('es-CL', { day: '2-digit', month: '2-digit', year: 'numeric' }) +
    ' ' + f.toLocaleTimeString('es-CL', { hour: '2-digit', minute: '2-digit' });

  var filas = items.map(function (it) {
    var e = reg[it.id] || {}, ev = estadoVenc(e.v);
    var obs = !e.ok ? 'NO VERIFICADO' : (ev === 'vencido' ? 'VENCIDO' : (ev === 'pronto' ? 'Vence en 3 meses o menos' : ''));
    return '<tr' + (obs ? ' class="ojo"' : '') + '><td>' + (e.ok ? '&#10003;' : '&#9744;') + '</td><td>' + esc(it.med.n) +
      '</td><td>' + esc(it.r[1]) + ' ' + esc(it.r[2]) + '</td><td class="m">' + esc(e.l || '') +
      '</td><td class="m">' + esc(e.v || '') + '</td><td class="m">' + esc(e.c == null ? '' : e.c) +
      '</td><td>' + esc(obs) + '</td></tr>';
  }).join('');

  var hechos = items.filter(function (it) { return reg[it.id] && reg[it.id].ok; }).length;
  var faltan = items.length - hechos;
  var w = window.open('', '_blank');
  if (!w) { alert('El navegador bloqueó la ventana del acta. Permitir ventanas emergentes para este sitio e intentar de nuevo.'); return; }
  w.document.write('<!DOCTYPE html><html lang="es-CL"><head><meta charset="utf-8">' +
    '<title>Acta de revisión — carro de paro</title><style>' +
    'body{font-family:Arial,Helvetica,sans-serif;font-size:11pt;color:#000;margin:22mm 16mm;line-height:1.4}' +
    'h1{font-size:14pt;margin:0 0 2px}h2{font-size:10pt;font-weight:normal;color:#444;margin:0 0 14px}' +
    'table{border-collapse:collapse;width:100%;font-size:9pt}th,td{border:1px solid #777;padding:3px 5px;text-align:left;vertical-align:top}' +
    'th{background:#e8eded}.m{font-family:Consolas,monospace}tr.ojo td{background:#fdf1ef}' +
    '.datos{margin:0 0 14px;font-size:10pt}.datos b{display:inline-block;min-width:132px}' +
    '.firmas{margin-top:34px;display:flex;gap:40px}.firmas div{flex:1;border-top:1px solid #000;padding-top:5px;font-size:9pt}' +
    '.pie{margin-top:16px;font-size:8.5pt;color:#555;line-height:1.4}' +
    '@media print{body{margin:14mm}}</style></head><body>' +
    '<h1>Acta de revisión del carro de paro</h1>' +
    '<h2>Arsenal Farmacológico APS — Resolución N.º 2504243202, SEREMI de Salud Región de Coquimbo, 11/09/2025</h2>' +
    '<div class="datos"><b>Establecimiento:</b> ' + esc(lugar) + '<br>' +
    '<b>Fecha y hora:</b> ' + esc(fecha) + '<br>' +
    '<b>Responsable:</b> ' + esc(resp) + '<br>' +
    '<b>Ítems verificados:</b> ' + hechos + ' de ' + items.length +
    (faltan ? ' <b>(' + faltan + ' sin verificar)</b>' : '') + '</div>' +
    '<table><thead><tr><th></th><th>Medicamento</th><th>Presentación</th><th>Lote</th><th>Vence</th><th>Cant.</th><th>Observación</th></tr></thead>' +
    '<tbody>' + filas + '</tbody></table>' +
    '<div class="firmas"><div>Firma de quien revisa</div><div>Firma de quien recibe o jefatura</div></div>' +
    '<div class="pie">Acta generada desde el Vademécum APS Coquimbo. El listado corresponde a las presentaciones ' +
    'consignadas para carro de paro en la resolución citada. La composición efectiva del carro debe ajustarse ' +
    'además a la normativa vigente de SAPU/SAR y a las instrucciones locales del establecimiento.</div>' +
    '</body></html>');
  w.document.close();
  setTimeout(function () { w.print(); }, 400);
}

/* ─────────────────────────── alertas ─────────────────────────── */

function pintarAlertas() {
  var h = '';

  /* vigencia de la resolución */
  var fin = new Date(2027, 8, 11);
  var dias = Math.round((fin - new Date()) / 86400000);
  h += '<div class="tarjeta"><h2>Vigencia de la resolución</h2>' +
    '<p style="margin:0;font-size:13.5px">La resolución fija una vigencia de <b>dos años</b> desde el 11/09/2025. ' +
    'Quedan <b class="mono">' + dias + '</b> días (hasta el <b>11/09/2027</b>). ' +
    'El Comité de Farmacia y Terapéutica del Servicio de Salud Coquimbo debe revisar y actualizar el arsenal cada dos años.</p></div>';

  function bloque(titulo, detalle, filtro, claseAviso) {
    var lista = [];
    D.meds.forEach(function (m) {
      m.p.forEach(function (r) { if (filtro(m, r)) lista.push({ m: m, r: r }); });
    });
    var vistos = {};
    lista = lista.filter(function (x) {
      var k = x.m.slug + '|' + x.r[0] + '|' + x.r[1] + '|' + x.r[2];
      if (vistos[k]) return false; vistos[k] = 1; return true;
    });
    if (!lista.length) return '';
    return '<div class="tarjeta"><h2>' + esc(titulo) + ' <span class="insignia ' + (claseAviso || '') + '">' + lista.length + '</span></h2>' +
      '<p style="margin:-4px 0 10px;font-size:12.5px;color:var(--tinta-2)">' + detalle + '</p>' +
      '<ul class="lista">' + lista.map(function (x) {
        return '<li><button class="fila" data-med="' + esc(x.m.slug) + '">' +
          '<span class="marca-estado"></span><span class="cuerpo">' +
          '<span class="nombre">' + esc(x.m.n) + '</span>' +
          '<span class="sub">' + esc(x.r[1]) + ' ' + esc(x.r[2]) + ' · ' + esc(ARS_CORTO[x.r[5]]) + '</span>' +
          '</span></button></li>';
      }).join('') + '</ul></div>';
  }

  h += bloque('Sin registro sanitario vigente',
    'Productos que la resolución identifica con registro sanitario no vigente o sin registro, distribuidos por CENABAST u otra vía autorizada. Verificar procedencia, trazabilidad y condiciones de conservación.',
    function (m, r) { return (r[7] || '').indexOf('Sin registro sanitario') >= 0; }, 'alerta');

  h += bloque('Sólo disponibles en hospitales',
    'La resolución restringe estos productos al nivel hospitalario. No corresponde dispensarlos en APS.',
    function (m, r) { return (r[7] || '').indexOf('Sólo hospitales') >= 0; }, 'alerta');

  h += bloque('Con alternativa terapéutica autorizada',
    'Ante quiebre de stock, la resolución ya autoriza un reemplazo para estos ítems. Abrí la ficha para ver cuál.',
    function (m, r) { return alternativas(r).length > 0; });

  h += bloque('Restringidos a SAPU o SAR',
    'Uso limitado a dispositivos de urgencia con la resolutividad correspondiente.',
    function (m, r) { return (r[7] || '').indexOf('SAPU/SAR') >= 0 || (r[7] || '').indexOf('Sólo SAR') >= 0; });

  $('#v-alertas').innerHTML = h;
}

/* ─────────────────────────── guía ─────────────────────────── */

function pintarGuia() {
  var h = '';

  h += '<div class="tarjeta"><h2>Cómo leer esta aplicación</h2>' +
    '<p style="margin:0 0 10px;font-size:13.5px">Elegí arriba tu establecimiento. Cada medicamento se muestra con ' +
    'una banda de color que responde una sola pregunta: <b>¿puedo entregarlo acá y bajo qué condición?</b></p>' +
    '<ul class="condiciones" style="border:1px solid var(--linea);border-radius:8px">' +
    '<li><span class="cl" style="color:var(--si)">Verde</span><span>Está en el arsenal de tu establecimiento, sin condiciones adicionales en la resolución.</span></li>' +
    '<li><span class="cl" style="color:var(--cond)">Ámbar</span><span>Está, pero con una condición: continuidad de tratamiento, programa o GES, registro sanitario, o restricción a SAPU/SAR.</span></li>' +
    '<li><span class="cl" style="color:var(--no)">Rojo</span><span>No figura en el arsenal de tu establecimiento, o la resolución lo restringe a hospitales.</span></li>' +
    '</ul></div>';

  h += '<div class="tarjeta"><h2>Qué es y qué no es</h2>' +
    '<p style="margin:0;font-size:13.5px">Esta aplicación reordena el arsenal aprobado por la resolución para poder ' +
    'consultarlo en terreno. <b>No es una fuente de dosis</b> ni reemplaza la prescripción, la ficha técnica vigente, ' +
    'los protocolos locales ni la evaluación clínica. El resumen clínico de cada ficha está escrito a nivel de grupo ' +
    'farmacológico, viene marcado como orientativo y no debe usarse como referencia farmacológica.</p></div>';

  h += '<div class="tarjeta"><h2>Uso sin conexión</h2>' +
    '<p style="margin:0 0 10px;font-size:13.5px">Después de la primera carga, la aplicación queda guardada en el ' +
    'teléfono o computador y funciona sin señal. Instalala desde el menú del navegador («Agregar a la pantalla de ' +
    'inicio» o «Instalar aplicación») para abrirla como una app.</p>' +
    '<div class="acciones no-imprimir"><span id="ranuraInstalar2"></span>' +
    '<button class="btn secundario" id="btnActualizar">Buscar actualización</button></div>' +
    '<p id="estadoSW" style="margin:10px 0 0;font-size:12px;color:var(--tinta-3)"></p></div>';

  Object.keys(D.notas).forEach(function (a) {
    var n = D.notas[a];
    h += '<div class="tarjeta"><h2>Notas al pie — ' + esc(a) + '</h2><ul class="notas-pie">' +
      Object.keys(n).sort(function (x, y) { return x - y; }).map(function (k) {
        return '<li><span class="num">' + esc(k) + '</span><span>' + esc(n[k]) + '</span></li>';
      }).join('') + '</ul></div>';
  });

  h += '<div class="tarjeta"><h2>Siglas</h2><dl class="def">' +
    D.siglas.map(function (s) {
      return '<div><dt>' + esc(s[0]) + '</dt><dd>' + esc(s[1]) + '</dd></div>';
    }).join('') + '</dl></div>';

  h += '<div class="tarjeta"><h2>Fuentes</h2><dl class="def">' +
    D.fuentes.map(function (f) {
      return '<div><dt style="width:120px;font-family:var(--sans);font-size:12.5px">' + esc(f[0]) + '</dt><dd>' +
        esc(f[1]) + (/^https?:/.test(f[2]) ? '<br><a href="' + esc(f[2]) + '" target="_blank" rel="noopener">' + esc(f[2]) + '</a>' : '') +
        '</dd></div>';
    }).join('') + '</dl>' +
    '<p style="margin:12px 0 0;font-size:12px;color:var(--tinta-3)">Versión de la aplicación ' + esc(D.meta.version) +
    ' · ' + D.meta.medicamentos + ' medicamentos y productos · ' + D.meta.presentaciones + ' presentaciones.</p></div>';

  var cont = $('#v-guia');
  cont.innerHTML = h;
  cont.querySelector('#btnActualizar').addEventListener('click', buscarActualizacion);
  montarInstalar();
  estadoSW();
}

/* ─────────────────────────── enrutado ─────────────────────────── */

function ir(ruta, med) {
  S.ruta = ruta; if (med !== undefined) S.med = med;
  var destino = ruta === 'med' && med ? '#/med/' + med.slug : '#/' + ruta;
  if (location.hash === destino) pintar(); else location.hash = destino;
}

function desdeHash() {
  var h = (location.hash || '#/buscar').replace(/^#\/?/, '').split('/');
  if (h[0] === 'med' && porSlug[h[1]]) { S.ruta = 'med'; S.med = porSlug[h[1]]; }
  else if (['buscar', 'carro', 'alertas', 'guia'].indexOf(h[0]) >= 0) S.ruta = h[0];
  else S.ruta = 'buscar';
  pintar();
}
window.addEventListener('hashchange', desdeHash);

function pintar() {
  document.querySelectorAll('.vista').forEach(function (v) { v.classList.remove('activa'); });
  $('#v-' + S.ruta).classList.add('activa');
  document.querySelectorAll('nav.inferior button').forEach(function (b) {
    var act = b.getAttribute('data-ruta') === S.ruta || (S.ruta === 'med' && b.getAttribute('data-ruta') === 'buscar');
    if (act) b.setAttribute('aria-current', 'page'); else b.removeAttribute('aria-current');
  });
  $('#zonaBusqueda').classList.toggle('oculto', S.ruta !== 'buscar');

  if (S.ruta === 'buscar') pintarBuscar();
  else if (S.ruta === 'med') pintarMed();
  else if (S.ruta === 'carro') pintarCarro();
  else if (S.ruta === 'alertas') pintarAlertas();
  else if (S.ruta === 'guia') pintarGuia();
}

document.querySelectorAll('nav.inferior button').forEach(function (b) {
  b.addEventListener('click', function () { ir(b.getAttribute('data-ruta')); });
});
document.addEventListener('click', function (ev) {
  if (!ev.target || !ev.target.closest) return;
  var f = ev.target.closest('[data-med]');
  if (f) { var m = porSlug[f.getAttribute('data-med')]; if (m) ir('med', m); }
});

/* chips de filtro */
var CHIPS = [
  { k: 'estab', t: function () { return 'Sólo ' + ARS_CORTO[S.est].toLowerCase(); } },
  { k: 'paro', t: function () { return 'Carro de paro'; } },
  { k: 'ges', t: function () { return 'Programa o GES'; } },
  { k: 'alerta', t: function () { return 'Con alerta'; } }
];
function pintarChips() {
  $('#chipsFiltro').innerHTML = CHIPS.map(function (c) {
    return '<button class="chip" data-f="' + c.k + '" aria-pressed="' + (S.filtros[c.k] ? 'true' : 'false') + '">' +
      esc(c.t()) + '</button>';
  }).join('');
  $('#chipsFiltro').querySelectorAll('[data-f]').forEach(function (b) {
    b.addEventListener('click', function () {
      var k = b.getAttribute('data-f');
      S.filtros[k] = !S.filtros[k];
      pintarChips(); pintar();
    });
  });
}

var inputQ = $('#q'), btnLimpiar = $('#btnLimpiar');
inputQ.addEventListener('input', function () {
  S.q = inputQ.value;
  btnLimpiar.classList.toggle('oculto', !S.q);
  if (S.ruta !== 'buscar') ir('buscar'); else pintarBuscar();
});
btnLimpiar.addEventListener('click', function () {
  inputQ.value = ''; S.q = ''; btnLimpiar.classList.add('oculto'); inputQ.focus(); pintar();
});

/* ─────────────────── PWA: instalación y caché ─────────────────── */

var promptInstalar = null;
window.addEventListener('beforeinstallprompt', function (e) {
  e.preventDefault(); promptInstalar = e; montarInstalar();
});
function montarInstalar() {
  if (!promptInstalar) return;
  var r = document.getElementById('ranuraInstalar') || document.getElementById('ranuraInstalar2');
  if (!r || r.dataset.listo) return;
  r.dataset.listo = '1';
  r.innerHTML = '<div class="aviso-instalar no-imprimir"><span>Instalala en el teléfono y queda disponible sin señal.</span>' +
    '<button class="btn" id="btnInstalar">Instalar</button></div>';
  r.querySelector('#btnInstalar').addEventListener('click', function () {
    promptInstalar.prompt();
    promptInstalar.userChoice.then(function () { promptInstalar = null; r.innerHTML = ''; });
  });
}

function estadoSW() {
  var p = document.getElementById('estadoSW'); if (!p) return;
  if (!('serviceWorker' in navigator)) {
    p.textContent = 'Este navegador no guarda la aplicación para uso sin conexión, o el archivo se abrió directamente desde el disco. Igual funciona: los datos están dentro del archivo.';
    return;
  }
  navigator.serviceWorker.getRegistration().then(function (reg) {
    p.textContent = reg
      ? 'Guardada para uso sin conexión. Última verificación: ' + new Date().toLocaleString('es-CL') + '.'
      : 'Todavía no está guardada para uso sin conexión.';
  });
}
function buscarActualizacion() {
  if (!('serviceWorker' in navigator)) { estadoSW(); return; }
  navigator.serviceWorker.getRegistration().then(function (reg) {
    if (!reg) { location.reload(); return; }
    reg.update().then(function () {
      var p = document.getElementById('estadoSW');
      if (p) p.textContent = 'Verificado. Si había una versión nueva, se aplicará al volver a abrir la aplicación.';
    });
  });
}
if ('serviceWorker' in navigator && location.protocol.indexOf('http') === 0) {
  window.addEventListener('load', function () {
    navigator.serviceWorker.register('sw.js').catch(function () {});
  });
}

function pintarRed() {
  var p = $('#puntoRed');
  p.classList.toggle('offline', !navigator.onLine);
  p.title = navigator.onLine ? 'Con conexión' : 'Sin conexión — la aplicación funciona igual';
}
window.addEventListener('online', pintarRed);
window.addEventListener('offline', pintarRed);

/* ─────────────────────────── arranque ─────────────────────────── */
pintarChips();
pintarRed();
desdeHash();

})();
