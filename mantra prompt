<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UMKM PromptGen - Riset & Positioning Assistant</title>
    
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Load React & ReactDOM -->
    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    
    <!-- Load Babel untuk memproses JSX di browser -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <style>
        /* Kustomisasi scrollbar */
        .custom-scrollbar::-webkit-scrollbar {
            width: 8px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: transparent;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background-color: #475569;
            border-radius: 20px;
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 font-sans">
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useRef } = React;

        // --- KOMPONEN ICON (Menggantikan lucide-react agar portabel) ---
        const IconSearch = ({ className }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><circle cx="11" cy="11" r="8"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>
        );
        const IconTarget = ({ className }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><circle cx="12" cy="12" r="10"></circle><circle cx="12" cy="12" r="6"></circle><circle cx="12" cy="12" r="2"></circle></svg>
        );
        const IconCopy = ({ className }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path></svg>
        );
        const IconCheck = ({ className }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><polyline points="20 6 9 17 4 12"></polyline></svg>
        );
        const IconBriefcase = ({ className }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><rect x="2" y="7" width="20" height="14" rx="2" ry="2"></rect><path d="M16 21V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v16"></path></svg>
        );
        const IconMessageSquare = ({ className }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"></path></svg>
        );
        const IconLightbulb = ({ className }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><path d="M15 14c.2-1 .7-1.7 1.5-2.5 1-.9 1.5-2.2 1.5-3.5A6 6 0 0 0 6 8c0 1.3.5 2.6 1.5 3.5.8.8 1.3 1.5 1.5 2.5"></path><path d="M9 18h6"></path><path d="M10 22h4"></path></svg>
        );
        const IconChevronRight = ({ className }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className}><polyline points="9 18 15 12 9 6"></polyline></svg>
        );


        // --- DATA TEMPLATE PROMPT ---
        const promptTemplates = [
        {
            id: 'mr-competitor',
            category: 'Riset Market',
            icon: <IconSearch className="w-5 h-5 text-blue-500" />,
            title: 'Analisis Kompetitor Area',
            description: 'Pahami peta persaingan dan cari celah (niche) yang bisa dimanfaatkan di area Anda.',
            fields: [
            { id: 'produk', label: 'Produk/Jasa yang dijual', placeholder: 'Contoh: Kopi Susu Gula Aren' },
            { id: 'lokasi', label: 'Lokasi Bisnis / Area Target', placeholder: 'Contoh: Jakarta Selatan' }
            ],
            generate: (data) => `Saya sedang menjalankan UMKM yang menjual ${data.produk || '[Produk]'}. Target operasional saya berada di area ${data.lokasi || '[Lokasi]'}. \n\nTolong bertindak sebagai Konsultan Bisnis dan buatkan riset pasar singkat yang mencakup:\n1. 3 profil imajiner kompetitor utama di area ini beserta kekuatan & kelemahan mereka.\n2. Analisis peluang (celah pasar) yang belum banyak digarap oleh kompetitor tersebut.\n3. Rekomendasi 3 inovasi sederhana yang bisa saya terapkan agar produk saya lebih menonjol.`
        },
        {
            id: 'mr-audience',
            category: 'Riset Market',
            icon: <IconTarget className="w-5 h-5 text-indigo-500" />,
            title: 'Pembuatan Buyer Persona',
            description: 'Kenali siapa sebenarnya pelanggan ideal yang paling mungkin membeli produk Anda.',
            fields: [
            { id: 'produk', label: 'Produk/Jasa', placeholder: 'Contoh: Skincare Organik' },
            { id: 'harga', label: 'Rentang Harga', placeholder: 'Contoh: Rp 50.000 - Rp 150.000' },
            { id: 'masalah', label: 'Masalah yang diselesaikan produk', placeholder: 'Contoh: Kulit kusam dan berjerawat' }
            ],
            generate: (data) => `Bisnis UMKM saya memproduksi ${data.produk || '[Produk]'} dengan rentang harga ${data.harga || '[Rentang Harga]'}. Produk ini diciptakan untuk menyelesaikan masalah: ${data.masalah || '[Masalah Pelanggan]'}.\n\nTolong buatkan 2 "Buyer Persona" (profil pelanggan ideal) yang sangat spesifik untuk produk ini. Untuk masing-masing persona, jabarkan:\n- Demografi (Umur, Pekerjaan, Pendapatan)\n- Psikografi (Minat, Gaya Hidup, Nilai yang dianut)\n- Pain Points (Ketakutan atau masalah utama mereka)\n- Bagaimana produk saya bisa menjadi pahlawan (solusi) untuk mereka.`
        },
        {
            id: 'bp-usp',
            category: 'Brand Positioning',
            icon: <IconLightbulb className="w-5 h-5 text-yellow-500" />,
            title: 'Merumuskan USP (Keunggulan Unik)',
            description: 'Temukan alasan kuat mengapa pelanggan harus memilih Anda dibanding yang lain.',
            fields: [
            { id: 'produk', label: 'Produk/Jasa', placeholder: 'Contoh: Jasa Cuci Sepatu' },
            { id: 'keunggulan1', label: 'Kelebihan 1 (Klaim Anda)', placeholder: 'Contoh: Pengerjaan hanya 1 hari' },
            { id: 'keunggulan2', label: 'Kelebihan 2 (Klaim Anda)', placeholder: 'Contoh: Pakai sabun khusus anti bakteri' }
            ],
            generate: (data) => `Saya memiliki usaha ${data.produk || '[Produk]'}. Saya merasa keunggulan utama bisnis saya dibandingkan kompetitor adalah: 1) ${data.keunggulan1 || '[Keunggulan 1]'} dan 2) ${data.keunggulan2 || '[Keunggulan 2]'}.\n\nTolong bantu saya merumuskan Unique Selling Proposition (USP) yang kuat. Berikan:\n1. 3 Pilihan kalimat USP yang padat, menarik, dan mudah diingat (maksimal 2 kalimat per pilihan).\n2. Slogan singkat (tagline) dari masing-masing USP tersebut.\n3. Saran bagaimana cara mengkomunikasikan USP ini di bio Instagram bisnis saya.`
        },
        {
            id: 'bp-voice',
            category: 'Brand Positioning',
            icon: <IconMessageSquare className="w-5 h-5 text-green-500" />,
            title: 'Menentukan Brand Voice & Gaya Bahasa',
            description: 'Buat pedoman cara brand Anda berbicara dan berinteraksi di media sosial.',
            fields: [
            { id: 'brand', label: 'Nama Brand', placeholder: 'Contoh: NgemilYuk' },
            { id: 'target', label: 'Target Audiens', placeholder: 'Contoh: Anak sekolah dan mahasiswa' },
            { id: 'sifat', label: 'Kesan Brand yang Diinginkan', placeholder: 'Contoh: Asik, gaul, receh, tapi terpercaya' }
            ],
            generate: (data) => `Nama brand UMKM saya adalah ${data.brand || '[Nama Brand]'}. Target pasar utama kami adalah ${data.target || '[Target Audiens]'}. Saya ingin brand ini memiliki sifat/karakter yang ${data.sifat || '[Kesan Brand]'}.\n\nSebagai ahli Branding, tolong buatkan panduan Brand Voice (Gaya Komunikasi) yang mencakup:\n1. Aturan Do's and Don'ts (Apa yang boleh dan tidak boleh dikatakan/dilakukan saat membalas chat atau bikin caption).\n2. Panggilan yang cocok dari brand ke audiens (misal: Sobat, Kak, Min, dll).\n3. Buatkan 2 contoh caption Instagram (1 untuk edukasi produk, 1 untuk promosi diskon) menggunakan gaya bahasa tersebut.`
        }
        ];

        // --- KOMPONEN UTAMA APLIKASI ---
        function App() {
            const [selectedTemplate, setSelectedTemplate] = useState(promptTemplates[0]);
            const [formData, setFormData] = useState({});
            const [copied, setCopied] = useState(false);
            const resultRef = useRef(null);

            const marketResearchTemplates = promptTemplates.filter(t => t.category === 'Riset Market');
            const brandPositioningTemplates = promptTemplates.filter(t => t.category === 'Brand Positioning');

            const handleTemplateClick = (template) => {
                setSelectedTemplate(template);
                setFormData({});
                setCopied(false);
            };

            const handleInputChange = (e, fieldId) => {
                setFormData({
                    ...formData,
                    [fieldId]: e.target.value
                });
                setCopied(false);
            };

            const handleCopy = () => {
                const textToCopy = selectedTemplate.generate(formData);
                const textArea = document.createElement("textarea");
                textArea.value = textToCopy;
                document.body.appendChild(textArea);
                textArea.select();
                try {
                    document.execCommand('copy');
                    setCopied(true);
                    setTimeout(() => setCopied(false), 2000);
                } catch (err) {
                    console.error('Failed to copy', err);
                }
                document.body.removeChild(textArea);
            };

            return (
                <div className="min-h-screen pb-12">
                    {/* Header */}
                    <header className="bg-white border-b border-slate-200 sticky top-0 z-10 shadow-sm">
                        <div className="max-w-6xl mx-auto px-4 py-4 sm:px-6 lg:px-8 flex items-center justify-between">
                            <div className="flex items-center gap-3">
                                <div className="bg-blue-600 p-2.5 rounded-xl shadow-sm">
                                    <IconBriefcase className="w-6 h-6 text-white" />
                                </div>
                                <div>
                                    <h1 className="text-xl font-bold text-slate-900 leading-tight">UMKM PromptGen</h1>
                                    <p className="text-sm text-slate-500 font-medium">Riset & Positioning Assistant</p>
                                </div>
                            </div>
                        </div>
                    </header>

                    <main className="max-w-6xl mx-auto px-4 py-8 sm:px-6 lg:px-8">
                        <div className="flex flex-col lg:flex-row gap-8">
                            
                            {/* Sidebar - Template Selection */}
                            <div className="w-full lg:w-1/3 space-y-8">
                                <div>
                                    <h2 className="text-sm font-bold tracking-wider text-slate-400 uppercase mb-4 px-2">Kategori: Riset Market</h2>
                                    <div className="space-y-3">
                                        {marketResearchTemplates.map(template => (
                                            <button
                                                key={template.id}
                                                onClick={() => handleTemplateClick(template)}
                                                className={`w-full text-left p-4 rounded-xl border transition-all duration-200 flex items-start gap-4 ${
                                                    selectedTemplate.id === template.id 
                                                        ? 'bg-blue-50 border-blue-300 shadow-md ring-1 ring-blue-500' 
                                                        : 'bg-white border-slate-200 hover:border-blue-300 hover:bg-slate-50 hover:shadow-sm'
                                                }`}
                                            >
                                                <div className="mt-1 bg-white p-2 rounded-lg shadow-sm border border-slate-100">
                                                    {template.icon}
                                                </div>
                                                <div className="flex-1">
                                                    <h3 className={`font-semibold ${selectedTemplate.id === template.id ? 'text-blue-900' : 'text-slate-800'}`}>
                                                        {template.title}
                                                    </h3>
                                                    <p className="text-xs text-slate-500 mt-1.5 leading-relaxed">{template.description}</p>
                                                </div>
                                            </button>
                                        ))}
                                    </div>
                                </div>

                                <div>
                                    <h2 className="text-sm font-bold tracking-wider text-slate-400 uppercase mb-4 px-2">Kategori: Brand Positioning</h2>
                                    <div className="space-y-3">
                                        {brandPositioningTemplates.map(template => (
                                            <button
                                                key={template.id}
                                                onClick={() => handleTemplateClick(template)}
                                                className={`w-full text-left p-4 rounded-xl border transition-all duration-200 flex items-start gap-4 ${
                                                    selectedTemplate.id === template.id 
                                                        ? 'bg-blue-50 border-blue-300 shadow-md ring-1 ring-blue-500' 
                                                        : 'bg-white border-slate-200 hover:border-blue-300 hover:bg-slate-50 hover:shadow-sm'
                                                }`}
                                            >
                                                <div className="mt-1 bg-white p-2 rounded-lg shadow-sm border border-slate-100">
                                                    {template.icon}
                                                </div>
                                                <div className="flex-1">
                                                    <h3 className={`font-semibold ${selectedTemplate.id === template.id ? 'text-blue-900' : 'text-slate-800'}`}>
                                                        {template.title}
                                                    </h3>
                                                    <p className="text-xs text-slate-500 mt-1.5 leading-relaxed">{template.description}</p>
                                                </div>
                                            </button>
                                        ))}
                                    </div>
                                </div>
                            </div>

                            {/* Main Content - Editor & Output */}
                            <div className="w-full lg:w-2/3 space-y-6">
                                
                                {/* Form Card */}
                                <div className="bg-white rounded-2xl border border-slate-200 shadow-sm overflow-hidden">
                                    <div className="border-b border-slate-100 px-6 py-5 bg-slate-50/80">
                                        <h2 className="text-lg font-bold text-slate-800 flex items-center gap-2">
                                            Kustomisasi Prompt
                                            <IconChevronRight className="w-4 h-4 text-slate-400" />
                                            <span className="text-blue-600">{selectedTemplate.title}</span>
                                        </h2>
                                        <p className="text-sm text-slate-500 mt-1">Isi detail bisnis Anda di bawah ini agar prompt yang dihasilkan spesifik.</p>
                                    </div>
                                    
                                    <div className="p-6 space-y-5">
                                        {selectedTemplate.fields.map(field => (
                                            <div key={field.id} className="space-y-2">
                                                <label htmlFor={field.id} className="block text-sm font-semibold text-slate-700">
                                                    {field.label}
                                                </label>
                                                <input
                                                    type="text"
                                                    id={field.id}
                                                    placeholder={field.placeholder}
                                                    value={formData[field.id] || ''}
                                                    onChange={(e) => handleInputChange(e, field.id)}
                                                    className="w-full px-4 py-3 rounded-xl border border-slate-300 focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition-colors bg-slate-50 focus:bg-white text-sm"
                                                />
                                            </div>
                                        ))}
                                    </div>
                                </div>

                                {/* Result Card */}
                                <div className="bg-slate-900 rounded-2xl shadow-xl overflow-hidden flex flex-col border border-slate-800">
                                    <div className="flex items-center justify-between px-6 py-4 border-b border-slate-800 bg-slate-900/80 backdrop-blur-sm">
                                        <h3 className="text-sm font-semibold text-white flex items-center gap-2">
                                            <div className="w-2 h-2 rounded-full bg-green-400 animate-pulse shadow-[0_0_8px_rgba(74,222,128,0.6)]"></div>
                                            Hasil Prompt (Siap Disalin)
                                        </h3>
                                        <button
                                            onClick={handleCopy}
                                            className={`flex items-center gap-2 px-4 py-2 rounded-xl text-sm font-bold transition-all ${
                                                copied 
                                                    ? 'bg-green-500/20 text-green-400 border border-green-500/30' 
                                                    : 'bg-white/10 text-white hover:bg-white/20 border border-white/10 hover:shadow-md'
                                            }`}
                                        >
                                            {copied ? <IconCheck className="w-4 h-4" /> : <IconCopy className="w-4 h-4" />}
                                            {copied ? 'Tersalin!' : 'Salin Prompt'}
                                        </button>
                                    </div>
                                    
                                    <div className="p-6 bg-slate-800/40 flex-1 relative group">
                                        <textarea
                                            ref={resultRef}
                                            readOnly
                                            value={selectedTemplate.generate(formData)}
                                            className="w-full h-full min-h-[280px] bg-transparent text-slate-200 text-sm leading-relaxed resize-none focus:outline-none placeholder-slate-600 custom-scrollbar"
                                        />
                                    </div>
                                    <div className="px-6 py-4 bg-slate-900 border-t border-slate-800/80">
                                        <p className="text-xs text-slate-400 flex items-start gap-2">
                                            <IconLightbulb className="w-4 h-4 text-yellow-500 flex-shrink-0" />
                                            <span><span className="font-semibold text-blue-400">Tips:</span> Salin teks di atas dan tempelkan ke AI Assistant (seperti ChatGPT, Claude, atau Gemini) untuk mendapatkan hasil riset & strategi Anda.</span>
                                        </p>
                                    </div>
                                </div>

                            </div>
                        </div>
                    </main>
                </div>
            );
        }

        // --- RENDER APLIKASI ---
        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
