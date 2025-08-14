import React, { useState, useEffect } from 'react';

/**
 * Delgado Baseball Training — Single-Page Site
 * Gold primary, black background, white text, red hover accents
 *
 * HOW TO USE
 * 1) Create a React (or Next.js) app
 * 2) Put your images into /public/dbt/ (e.g., /public/dbt/logo.png, /public/dbt/huddle.jpeg)
 * 3) Replace image paths below with /dbt/<filename>
 * 4) Replace CALENDLY/EMAIL/PHONE
 * 5) Export this component as the default page (e.g., App.jsx or app/page.tsx)
 */

const BRAND = {
  black: '#0B0B0B',
  white: '#FFFFFF',
  red: '#E53935',
  gold: '#C9A227',
};

const CALENDLY = 'https://calendly.com/delgadobaseball/assessment'; // <- replace
const EMAIL = 'hello@delgadobaseball.com'; // <- replace
const PHONE = '(555) 555-0123'; // <- replace

// --- GALLERY IMAGES ---
// Add more by pushing to this array; they will appear automatically in Gallery
const IMAGES = [
  { src: '/dbt/huddle.jpeg', alt: 'Nighttime huddle' },
  // { src: '/dbt/photo1.jpeg', alt: 'Description' },
  // { src: '/dbt/photo2.jpeg', alt: 'Description' },
];

function GoldButton({ children, href, onClick, style = {}, large = false }) {
  const [hover, setHover] = useState(false);
  const base = {
    backgroundColor: hover ? BRAND.red : BRAND.gold,
    color: BRAND.black,
    padding: large ? '1rem 1.5rem' : '0.75rem 1rem',
    border: 'none',
    borderRadius: 8,
    fontWeight: 800,
    cursor: 'pointer',
    textDecoration: 'none',
    display: 'inline-block',
    transition: 'background-color .2s ease',
    ...style,
  };
  if (href) {
    return (
      <a
        href={href}
        onMouseEnter={() => setHover(true)}
        onMouseLeave={() => setHover(false)}
        style={base}
      >
        {children}
      </a>
    );
  }
  return (
    <button
      onClick={onClick}
      onMouseEnter={() => setHover(true)}
      onMouseLeave={() => setHover(false)}
      style={base}
    >
      {children}
    </button>
  );
}

const Section = ({ id, children, style = {} }) => (
  <section id={id} style={{ maxWidth: 1200, margin: '0 auto', padding: '64px 20px', ...style }}>
    {children}
  </section>
);

const Card = ({ children }) => (
  <div style={{ background: '#111', border: `1px solid ${BRAND.gold}22`, borderRadius: 16, padding: 20 }}>
    {children}
  </div>
);

// --- LIGHTBOX COMPONENT with Next/Prev + Keyboard Controls ---
function Lightbox({ open, index, onClose, onPrev, onNext }) {
  useEffect(() => {
    if (!open) return;
    const onKey = (e) => {
      if (e.key === 'Escape') onClose();
      if (e.key === 'ArrowLeft') onPrev();
      if (e.key === 'ArrowRight') onNext();
    };
    window.addEventListener('keydown', onKey);
    return () => window.removeEventListener('keydown', onKey);
  }, [open, onClose, onPrev, onNext]);

  if (!open) return null;
  return (
    <div
      onClick={onClose}
      style={{
        position: 'fixed',
        inset: 0,
        background: 'rgba(0,0,0,.9)',
        display: 'flex',
        alignItems: 'center',
        justifyContent: 'center',
        zIndex: 100,
      }}
    >
      <div onClick={(e) => e.stopPropagation()} style={{ position: 'relative' }}>
        <img
          src={IMAGES[index].src}
          alt={IMAGES[index].alt}
          style={{ maxWidth: '92vw', maxHeight: '88vh', borderRadius: 12, border: `2px solid ${BRAND.gold}` }}
        />
        {/* Prev */}
        <button
          aria-label="Previous"
          onClick={onPrev}
          style={{
            position: 'absolute',
            top: '50%',
            left: -60,
            transform: 'translateY(-50%)',
            background: BRAND.gold,
            color: BRAND.black,
            border: 'none',
            borderRadius: 999,
            width: 44,
            height: 44,
            fontWeight: 900,
            cursor: 'pointer',
          }}
        >
          {'‹'}
        </button>
        {/* Next */}
        <button
          aria-label="Next"
          onClick={onNext}
          style={{
            position: 'absolute',
            top: '50%',
            right: -60,
            transform: 'translateY(-50%)',
            background: BRAND.gold,
            color: BRAND.black,
            border: 'none',
            borderRadius: 999,
            width: 44,
            height: 44,
            fontWeight: 900,
            cursor: 'pointer',
          }}
        >
          {'›'}
        </button>
        {/* Close */}
        <button
          aria-label="Close"
          onClick={onClose}
          style={{
            position: 'absolute',
            top: -56,
            right: 0,
            background: BRAND.red,
            color: BRAND.white,
            border: 'none',
            borderRadius: 8,
            padding: '6px 10px',
            fontWeight: 800,
            cursor: 'pointer',
          }}
        >
          Close ✕
        </button>
      </div>
    </div>
  );
}

export default function DelgadoBaseballTraining() {
  const [lightbox, setLightbox] = useState({ open: false, index: 0 });
  const openAt = (i) => setLightbox({ open: true, index: i });
  const close = () => setLightbox({ open: false, index: 0 });
  const prev = () => setLightbox((s) => ({ open: true, index: (s.index - 1 + IMAGES.length) % IMAGES.length }));
  const next = () => setLightbox((s) => ({ open: true, index: (s.index + 1) % IMAGES.length }));

  return (
    <div
      style={{
        backgroundColor: BRAND.black,
        color: BRAND.white,
        fontFamily: 'ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto',
        minHeight: '100vh',
      }}
    >
      {/* NAVIGATION */}
      <nav
        style={{
          position: 'sticky',
          top: 0,
          zIndex: 50,
          background: '#0B0B0BE6',
          backdropFilter: 'blur(6px)',
          borderBottom: `2px solid ${BRAND.gold}`,
        }}
      >
        <div
          style={{
            maxWidth: 1200,
            margin: '0 auto',
            padding: '14px 20px',
            display: 'flex',
            alignItems: 'center',
            justifyContent: 'space-between',
          }}
        >
          <div style={{ display: 'flex', alignItems: 'center', gap: 12 }}>
            <img src="/dbt/logo.png" alt="Delgado Baseball Training" style={{ height: 40 }} />
            <strong style={{ color: BRAND.gold, letterSpacing: 0.5 }}>Delgado Baseball Training</strong>
          </div>
          <div style={{ display: 'flex', gap: 18, alignItems: 'center' }}>
            <a href="#programs" style={{ color: BRAND.white, textDecoration: 'none' }}>
              Programs
            </a>
            <a href="#pricing" style={{ color: BRAND.white, textDecoration: 'none' }}>
              Memberships
            </a>
            <a href="#gallery" style={{ color: BRAND.white, textDecoration: 'none' }}>
              Gallery
            </a>
            <a href="#results" style={{ color: BRAND.white, textDecoration: 'none' }}>
              Results
            </a>
            <a href="#contact" style={{ color: BRAND.white, textDecoration: 'none' }}>
              Contact
            </a>
            <GoldButton href={CALENDLY}>Book Assessment</GoldButton>
          </div>
        </div>
      </nav>

      {/* HERO — Nighttime Huddle */}
      <Section id="hero">
        <div style={{ display: 'grid', gridTemplateColumns: '1.2fr 1fr', gap: 24 }}>
          <div>
            <div
              style={{
                display: 'inline-block',
                padding: '6px 10px',
                borderRadius: 999,
                background: BRAND.gold,
                color: BRAND.black,
                fontWeight: 800,
                marginBottom: 12,
              }}
            >
              NYC • Year‑Round Training
            </div>
            <h1 style={{ fontSize: 44, lineHeight: 1.1, margin: 0, fontWeight: 900 }}>
              Train Like a Collegiate Athlete. <span style={{ color: BRAND.gold }}>Play Above Your Level.</span>
            </h1>
            <p style={{ marginTop: 12, opacity: 0.9, fontSize: 18, maxWidth: 640 }}>
              Elite baseball performance in New York City: strength, speed, hitting, throwing and on‑field IQ. Proven
              programs for infielders, outfielders, pitchers and catchers.
            </p>
            <div style={{ display: 'flex', gap: 12, marginTop: 16, flexWrap: 'wrap' }}>
              <GoldButton href={CALENDLY} large>
                Start Free Assessment
              </GoldButton>
              <GoldButton
                href="#programs"
                large
                style={{ backgroundColor: 'transparent', color: BRAND.gold, border: `2px solid ${BRAND.gold}` }}
              >
                Explore Programs
              </GoldButton>
            </div>
          </div>
          <div>
            <div style={{ position: 'relative' }}>
              <img
                src={IMAGES[0].src}
                alt={IMAGES[0].alt}
                style={{ width: '100%', borderRadius: 24, boxShadow: '0 20px 60px rgba(0,0,0,.5)', objectFit: 'cover', maxHeight: 520 }}
              />
              <div
                style={{
                  position: 'absolute',
                  bottom: -14,
                  left: -14,
                  background: '#111',
                  border: `1px solid ${BRAND.gold}`,
                  borderRadius: 14,
                  padding: '10px 14px',
                }}
              >
                <strong>Performance Lab</strong>
                <div style={{ fontSize: 12, opacity: 0.8 }}>Strength • Speed • Mobility</div>
              </div>
            </div>
          </div>
        </div>
      </Section>

      {/* PROGRAMS */}
      <Section id="programs">
        <h2 style={{ fontSize: 32, fontWeight: 900, marginTop: 0 }}>Signature Programs</h2>
        <div style={{ display: 'grid', gridTemplateColumns: 'repeat(4, minmax(0, 1fr))', gap: 16 }}>
          {[
            {
              title: 'Hitting Power & Consistency',
              desc: 'Mechanics, bat speed, approach and contact quality for game‑day results.',
              points: ['EV & bat speed tracking', 'Approach & pitch recognition', 'On‑plane efficiency drills'],
            },
            {
              title: 'Pitcher Velocity & Arm Care',
              desc: 'Sustainable velo gains with mechanics, mobility and recovery.',
              points: ['Velo progressions', 'Decel & mobility work', 'Recovery protocols'],
            },
            {
              title: 'Infield/Outfield Defense IQ',
              desc: 'Reads, routes, transfers and footwork to convert more outs.',
              points: ['First‑step quickness', 'Angles & routes', 'Transfer speed'],
            },
            {
              title: 'Speed & Strength Systems',
              desc: 'Linear speed, lateral quickness and baseball‑specific strength.',
              points: ['60‑yd sprint prep', 'Plyometrics & power', 'Rotational strength'],
            },
          ].map((p) => (
            <Card key={p.title}>
              <h3 style={{ margin: 0, fontWeight: 800 }}>{p.title}</h3>
              <p style={{ opacity: 0.85 }}>{p.desc}</p>
              <ul style={{ paddingLeft: 18, margin: '8px 0' }}>
                {p.points.map((x) => (
                  <li key={x} style={{ marginBottom: 4 }}>
                    {x}
                  </li>
                ))}
              </ul>
            </Card>
          ))}
        </div>
      </Section>

      {/* PRICING */}
      <Section id="pricing">
        <h2 style={{ fontSize: 32, fontWeight: 900, marginTop: 0 }}>Memberships & Packages</h2>
        <div style={{ display: 'grid', gridTemplateColumns: 'repeat(3, 1fr)', gap: 16 }}>
          {[
            {
              name: 'Starter — 1:1',
              price: '$89',
              period: 'per session',
              features: ['60‑min private session', 'Movement screen', 'Personalized drills'],
            },
            {
              name: 'Squad — Small Group',
              price: '$49',
              period: 'per athlete / session',
              features: ['4‑6 athletes', 'Strength & speed block', 'Competition games'],
              popular: true,
            },
            {
              name: 'Unlimited Monthly',
              price: '$279',
              period: 'per month',
              features: ['Unlimited group sessions', 'Monthly testing report', 'Priority event access'],
            },
          ].map((plan) => (
            <div
              key={plan.name}
              style={{
                border: `2px solid ${plan.popular ? BRAND.gold : BRAND.gold}55`,
                borderRadius: 16,
                padding: 20,
                position: 'relative',
                background: '#111',
              }}
            >
              {plan.popular && (
                <div
                  style={{
                    position: 'absolute',
                    top: -12,
                    left: 16,
                    background: BRAND.gold,
                    color: BRAND.black,
                    fontWeight: 900,
                    padding: '4px 8px',
                    borderRadius: 8,
                  }}
                >
                  Most Popular
                </div>
              )}
              <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'baseline' }}>
                <h3 style={{ margin: 0 }}>{plan.name}</h3>
                <div>
                  <div style={{ fontSize: 28, fontWeight: 900, color: BRAND.gold }}>{plan.price}</div>
                  <div style={{ fontSize: 12, opacity: 0.7 }}>{plan.period}</div>
                </div>
              </div>
              <ul style={{ paddingLeft: 18, margin: '12px 0' }}>
                {plan.features.map((f) => (
                  <li key={f} style={{ marginBottom: 6 }}>
                    {f}
                  </li>
                ))}
              </ul>
              <GoldButton href={CALENDLY} style={{ width: '100%', textAlign: 'center' }}>
                Get Started
              </GoldButton>
            </div>
          ))}
        </div>
        <p style={{ fontSize: 12, opacity: 0.7, marginTop: 8 }}>
          * Pricing is placeholder — adjust to your market. Team packages and college prep available.
        </p>
      </Section>

      {/* GALLERY with Lightbox */}
      <Section id="gallery">
        <h2 style={{ fontSize: 32, fontWeight: 900, marginTop: 0 }}>Gallery</h2>
        <p style={{ opacity: 0.85, marginTop: 6 }}>Real Delgado Baseball Training moments: practices, games, and athlete development.</p>
        {IMAGES.length === 0 ? (
          <p style={{ opacity: 0.7 }}>Add images to the IMAGES array to populate this gallery.</p>
        ) : (
          <div style={{ display: 'grid', gridTemplateColumns: 'repeat(3, 1fr)', gap: 12, marginTop: 16 }}>
            {IMAGES.map((img, i) => (
              <img
                key={img.src}
                src={img.src}
                alt={img.alt}
                onClick={() => openAt(i)}
                style={{
                  width: '100%',
                  height: 190,
                  objectFit: 'cover',
                  cursor: 'pointer',
                  borderRadius: 12,
                  border: `1px solid ${BRAND.gold}22`,
                }}
              />
            ))}
          </div>
        )}
      </Section>

      {/* RESULTS */}
      <Section id="results">
        <h2 style={{ fontSize: 32, fontWeight: 900, marginTop: 0 }}>Results & Testimonials</h2>
        <div style={{ display: 'grid', gridTemplateColumns: 'repeat(3, 1fr)', gap: 16 }}>
          {[
            { quote: 'Dropped my 60 from 7.4 to 6.9 in 10 weeks.', name: 'Eddie F.', role: '2026 SS/OF' },
            { quote: '+4 mph on the mound and healthier arm.', name: 'Kaden C.', role: '2027 RHP' },
            { quote: 'Best mix of coaching, culture and accountability.', name: 'Alex G.', role: 'Parent' },
          ].map((t) => (
            <Card key={t.name}>
              <p style={{ fontSize: 18, lineHeight: 1.5, margin: 0 }}>“{t.quote}”</p>
              <div style={{ marginTop: 8, fontSize: 13, opacity: 0.75 }}>— {t.name}, {t.role}</div>
            </Card>
          ))}
        </div>
      </Section>

      {/* LOCATION */}
      <Section id="location">
        <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: 16, alignItems: 'center' }}>
          <div>
            <h2 style={{ fontSize: 32, fontWeight: 900, marginTop: 0 }}>Location & Hours</h2>
            <p style={{ opacity: 0.85 }}>
              Based in New York City. Mobile sessions available. Facility partnerships in the Bronx and Westchester.
            </p>
            <ul style={{ paddingLeft: 18 }}>
              <li>NYC • Bronx • Westchester</li>
              <li>Mon–Fri 3‑9p • Sat 10a‑4p • Sun by appt</li>
              <li>{PHONE}</li>
              <li>{EMAIL}</li>
            </ul>
          </div>
          <div
            style={{
              borderRadius: 16,
              overflow: 'hidden',
              background: '#111',
              border: `1px solid ${BRAND.gold}22`,
              aspectRatio: '16/9',
              display: 'flex',
              alignItems: 'center',
              justifyContent: 'center',
            }}
          >
            <span style={{ opacity: 0.7 }}>Embed Google Map or facility video here</span>
          </div>
        </div>
      </Section>

      {/* CONTACT */}
      <Section id="contact">
        <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: 16 }}>
          <div>
            <h2 style={{ fontSize: 32, fontWeight: 900, marginTop: 0 }}>Book Your Free Assessment</h2>
            <p style={{ opacity: 0.85 }}>
              Tell us about your position, goals and current metrics. We’ll recommend a program and timeline to get you
              results.
            </p>
            <form
              onSubmit={(e) => {
                e.preventDefault();
                window.location.href = CALENDLY;
              }}
              style={{ display: 'grid', gap: 10, marginTop: 12 }}
            >
              <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: 10 }}>
                <input
                  placeholder="Athlete Name"
                  required
                  style={{ padding: 12, borderRadius: 8, border: `1px solid ${BRAND.gold}55`, background: '#0f0f0f', color: BRAND.white }}
                />
                <input
                  type="email"
                  placeholder="Parent/Contact Email"
                  required
                  style={{ padding: 12, borderRadius: 8, border: `1px solid ${BRAND.gold}55`, background: '#0f0f0f', color: BRAND.white }}
                />
              </div>
              <div style={{ display: 'grid', gridTemplateColumns: '1fr 1fr', gap: 10 }}>
                <input
                  placeholder="Position (e.g., OF, SS, RHP)"
                  style={{ padding: 12, borderRadius: 8, border: `1px solid ${BRAND.gold}55`, background: '#0f0f0f', color: BRAND.white }}
                />
                <input
                  placeholder="Graduation Year"
                  style={{ padding: 12, borderRadius: 8, border: `1px solid ${BRAND.gold}55`, background: '#0f0f0f', color: BRAND.white }}
                />
              </div>
              <textarea
                placeholder="Goals (e.g., +5 mph velo, +5 EV, sub‑7 60)"
                rows={4}
                style={{ padding: 12, borderRadius: 8, border: `1px solid ${BRAND.gold}55`, background: '#0f0f0f', color: BRAND.white }}
              />
              <div style={{ display: 'flex', gap: 10, alignItems: 'center' }}>
                <GoldButton large>Request Call</GoldButton>
                <GoldButton
                  href={CALENDLY}
                  large
                  style={{ backgroundColor: 'transparent', color: BRAND.gold, border: `2px solid ${BRAND.gold}` }}
                >
                  Schedule on Calendly
                </GoldButton>
              </div>
              <div style={{ fontSize: 12, opacity: 0.7 }}>
                * Form submits to Calendly by default. We can wire this to email/CRM instead.
              </div>
            </form>
          </div>
          <div>
            <Card>
              <h3 style={{ marginTop: 0 }}>Why Athletes Choose Delgado</h3>
              <ul style={{ paddingLeft: 18 }}>
                <li>Evidence‑based programming with progress testing</li>
                <li>College recruiting support & showcase prep</li>
                <li>Community, accountability and culture that wins</li>
              </ul>
              <div style={{ marginTop: 12 }}>
                <GoldButton href={`mailto:${EMAIL}`} style={{ marginRight: 10 }}>
                  Email Us
                </GoldButton>
                <GoldButton href={`tel:${PHONE.replace(/[^0-9+]/g, '')}`}>Call Us</GoldButton>
              </div>
            </Card>
          </div>
        </div>
      </Section>

      {/* FOOTER */}
      <footer style={{ borderTop: `2px solid ${BRAND.gold}`, background: '#0B0B0B', padding: '28px 20px' }}>
        <div
          style={{
            maxWidth: 1200,
            margin: '0 auto',
            display: 'grid',
            gridTemplateColumns: '2fr 1fr 1fr 1fr',
            gap: 16,
            alignItems: 'start',
          }}
        >
          <div>
            <div style={{ display: 'flex', alignItems: 'center', gap: 10, fontWeight: 800 }}>
              <img src="/dbt/logo.png" alt="DBT logo" style={{ height: 24 }} /> Delgado Baseball Training
            </div>
            <p style={{ opacity: 0.8 }}>Elite training for hitters, pitchers, infielders and outfielders in NYC and beyond.</p>
          </div>
          <div>
            <strong>Programs</strong>
            <ul style={{ listStyle: 'none', padding: 0, marginTop: 8 }}>
              <li>
                <a href="#programs" style={{ color: BRAND.white, textDecoration: 'none' }}>
                  Hitting
                </a>
              </li>
              <li>
                <a href="#programs" style={{ color: BRAND.white, textDecoration: 'none' }}>
                  Pitching
                </a>
              </li>
              <li>
                <a href="#programs" style={{ color: BRAND.white, textDecoration: 'none' }}>
                  Defense
                </a>
              </li>
              <li>
                <a href="#programs" style={{ color: BRAND.white, textDecoration: 'none' }}>
                  Speed & Strength
                </a>
              </li>
            </ul>
          </div>
          <div>
            <strong>Company</strong>
            <ul style={{ listStyle: 'none', padding: 0, marginTop: 8 }}>
              <li>
                <a href="#gallery" style={{ color: BRAND.white, textDecoration: 'none' }}>
                  Gallery
                </a>
              </li>
              <li>
                <a href="#results" style={{ color: BRAND.white, textDecoration: 'none' }}>
                  Results
                </a>
              </li>
              <li>
                <a href="#pricing" style={{ color: BRAND.white, textDecoration: 'none' }}>
                  Memberships
                </a>
              </li>
              <li>
                <a href="#contact" style={{ color: BRAND.white, textDecoration: 'none' }}>
                  Contact
                </a>
              </li>
            </ul>
          </div>
          <div>
            <strong>Contact</strong>
            <ul style={{ listStyle: 'none', padding: 0, marginTop: 8 }}>
              <li>{PHONE}</li>
              <li>{EMAIL}</li>
              <li>NYC • Bronx • Westchester</li>
            </ul>
          </div>
        </div>
        <div style={{ textAlign: 'center', marginTop: 20, opacity: 0.6, fontSize: 12 }}>
          © {new Date().getFullYear()} Delgado Baseball Training. All rights reserved.
        </div>
      </footer>

      {/* Lightbox Mount */}
      <Lightbox open={lightbox.open} index={lightbox.index} onClose={close} onPrev={prev} onNext={next} />
    </div>
  );
}
