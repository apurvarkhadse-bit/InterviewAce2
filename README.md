import React, { useState, useEffect } from 'react';
import { Link } from 'react-router-dom';
import { useAuth } from '../context/AuthContext';
import api from '../services/api';
import {
    Mic2,
    GraduationCap,
    FileText,
    Briefcase,
    Code2,
    Sparkles,
    TrendingUp,
    CheckCircle2,
    Circle,
    Award,
    ArrowRight,
    ShieldAlert,
    Flame,
    Star,
    Users,
    Volume2,
    Calendar,
    Building2,
    Clock,
    ExternalLink
} from 'lucide-react';

export default function Dashboard() {
    const { user } = useAuth();
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState('');
    

    const fetchDashboard = async () => {
        try {
            setLoading(true);
            const res = await api.getDashboardData();
            setData(res);
        } catch (err) {
            console.error("Dashboard fetch error:", err);
            setError('Unable to load dashboard data. Please make sure the backend is running.');
        } finally {
            setLoading(false);
        }
    };

    useEffect(() => {
        fetchDashboard();
    }, []);

    const handleToggleModule = async (moduleId) => {
        try {
            const res = await api.toggleLearningModule(moduleId);
            if (res.success && res.learningPath) {
                setData(prev => ({ ...prev, learningPath: res.learningPath }));
            }
        } catch (err) {
            console.error("Failed to toggle module:", err);
        }
    };

    if (loading) {
        return (
            <div className="p-8 flex flex-col items-center justify-center min-h-[60vh]">
                <div className="w-10 h-10 border-3 border-brand-500 border-t-transparent rounded-full animate-spin mb-4" />
                <p className="text-sm text-slate-400">Loading student analytics & learning milestones...</p>
            </div>
        );
    }

    const progressReport = data?.progressReport || { interviewsCompleted: 5, averageScore: 82, skillAssessmentsTaken: 3 };
    const learningPath = data?.learningPath || [];
    const jobTracker = data?.jobTracker || [];
    const codingChallenges = data?.codingChallenges || { solved: 14, total: 50 };
    const softSkillsScore = data?.softSkillsScore || 78;
    const peerFeedback = data?.peerFeedback || [];

    const completedModules = learningPath.filter(m => m.completed).length;
    const learningProgress = learningPath.length ? Math.round((completedModules / learningPath.length) * 100) : 0;

    return (
        <div className="p-4 sm:p-6 lg:p-8 max-w-7xl mx-auto space-y-8">
            {/* Welcome Banner */}
            <div className="relative overflow-hidden rounded-3xl bg-gradient-to-r from-brand-950 via-slate-900 to-indigo-950 p-6 sm:p-8 border border-brand-500/20 shadow-2xl">
                <div className="absolute top-0 right-0 -mt-10 -mr-10 w-80 h-80 bg-brand-500/10 rounded-full blur-3xl pointer-events-none" />
                <div className="relative z-10 flex flex-col md:flex-row md:items-center justify-between gap-6">
                    <div className="space-y-2">
                        <div className="inline-flex items-center gap-2 px-3 py-1 rounded-full text-xs font-semibold bg-brand-500/20 text-brand-300 border border-brand-500/30">
                            <Sparkles className="w-3.5 h-3.5 text-brand-400" />
                            <span>Placement Ready 2026</span>
                        </div>
                        <h1 className="text-2xl sm:text-3xl font-extrabold text-white tracking-tight">
                            Hello, {user?.name || 'Engineer'} 👋
                        </h1>
                        <p className="text-sm text-slate-300 max-w-xl leading-relaxed">
                            Target Specialization: <span className="font-semibold text-brand-400">{user?.targetBranch || 'Computer Science'}</span>.
                            Keep up the momentum—daily voice practice increases technical fluency by 3.4x.
                        </p>
                    </div>

                    <div className="flex flex-wrap items-center gap-3">
                        <Link
                            to="/interview"
                            className="inline-flex items-center gap-2 px-5 py-3 rounded-xl bg-gradient-to-r from-brand-600 to-indigo-600 hover:from-brand-500 hover:to-indigo-500 text-white text-sm font-bold shadow-lg shadow-brand-600/30 hover:shadow-brand-500/50 transition-all active:scale-95"
                        >
                            <Mic2 className="w-4 h-4" />
                            <span>Start AI Mock Interview</span>
                        </Link>
                        <Link
                            to="/skills"
                            className="inline-flex items-center gap-2 px-4 py-3 rounded-xl bg-slate-900/80 hover:bg-slate-800 text-slate-200 text-sm font-semibold border border-slate-700/80 transition-colors"
                        >
                            <GraduationCap className="w-4 h-4" />
                            <span>Take Skill Quiz</span>
                        </Link>
                    </div>
                </div>
            </div>

            {/* Metrics Row */}
            <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                {/* Metric 1 */}
                <div className="glass-panel p-5 rounded-2xl border border-slate-800/80 flex items-center justify-between hover:border-brand-500/30 transition-colors">
                    <div>
                        <p className="text-xs font-medium text-slate-400">Interviews Completed</p>
                        <h3 className="text-2xl font-black text-white mt-1">{progressReport.interviewsCompleted}</h3>
                        <span className="text-[11px] font-semibold text-emerald-400 flex items-center gap-1 mt-1">
                            <TrendingUp className="w-3 h-3" /> +2 this week
                        </span>
                    </div>
                    <div className="p-3.5 rounded-xl bg-brand-500/10 text-brand-400 border border-brand-500/20">
                        <Mic2 className="w-6 h-6" />
                    </div>
                </div>

                {/* Metric 2 */}
                <div className="glass-panel p-5 rounded-2xl border border-slate-800/80 flex items-center justify-between hover:border-emerald-500/30 transition-colors">
                    <div>
                        <p className="text-xs font-medium text-slate-400">Average NLP Score</p>
                        <h3 className="text-2xl font-black text-white mt-1">{progressReport.averageScore}%</h3>
                        <span className="text-[11px] font-semibold text-emerald-400 flex items-center gap-1 mt-1">
                            <TrendingUp className="w-3 h-3" /> High domain accuracy
                        </span>
                    </div>
                    <div className="p-3.5 rounded-xl bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">
                        <Award className="w-6 h-6" />
                    </div>
                </div>

                {/* Metric 3 */}
                <div className="glass-panel p-5 rounded-2xl border border-slate-800/80 flex items-center justify-between hover:border-purple-500/30 transition-colors">
                    <div>
                        <p className="text-xs font-medium text-slate-400">Soft Skills & Tone</p>
                        <h3 className="text-2xl font-black text-white mt-1">{softSkillsScore} / 100</h3>
                        <span className="text-[11px] font-semibold text-purple-400 flex items-center gap-1 mt-1">
                            <Volume2 className="w-3 h-3" /> 135 WPM (Optimal)
                        </span>
                    </div>
                    <div className="p-3.5 rounded-xl bg-purple-500/10 text-purple-400 border border-purple-500/20">
                        <Volume2 className="w-6 h-6" />
                    </div>
                </div>

                {/* Metric 4 */}
                <div className="glass-panel p-5 rounded-2xl border border-slate-800/80 flex items-center justify-between hover:border-amber-500/30 transition-colors">
                    <div>
                        <p className="text-xs font-medium text-slate-400">Coding Challenges</p>
                        <h3 className="text-2xl font-black text-white mt-1">{codingChallenges.solved} / {codingChallenges.total}</h3>
                        <span className="text-[11px] font-semibold text-amber-400 flex items-center gap-1 mt-1">
                            <Flame className="w-3 h-3" /> 6 day streak
                        </span>
                    </div>
                    <div className="p-3.5 rounded-xl bg-amber-500/10 text-amber-400 border border-amber-500/20">
                        <Code2 className="w-6 h-6" />
                    </div>
                </div>
            </div>

            {/* Main Grid: Learning Path + Job Pipeline */}
            <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
                {/* Left Column (2 Cols): Learning Path & Score Trend */}
                <div className="lg:col-span-2 space-y-8">
                    {/* Personalized Learning Path */}
                    <div className="glass-panel rounded-2xl p-6 border border-slate-800/80">
                        <div className="flex items-center justify-between mb-4">
                            <div>
                                <h2 className="text-lg font-bold text-white flex items-center gap-2">
                                    <CheckCircle2 className="w-5 h-5 text-brand-400" />
                                    <span>Personalized Learning Path</span>
                                </h2>
                                <p className="text-xs text-slate-400 mt-0.5">
                                    Milestones tailored for your target specialization
                                </p>
                            </div>
                            <span className="text-xs font-bold px-2.5 py-1 rounded-full bg-brand-500/15 text-brand-300 border border-brand-500/30">
                                {learningProgress}% Completed
                            </span>
                        </div>

                        {/* Progress bar */}
                        <div className="w-full h-2 rounded-full bg-slate-800 mb-6 overflow-hidden">
                            <div
                                className="h-full bg-gradient-to-r from-brand-500 to-indigo-500 rounded-full transition-all duration-500"
                                style={{ width: `${learningProgress}%` }}
                            />
                        </div>

                        {/* Modules list */}
                        <div className="space-y-3">
                            {learningPath.map((module) => (
                                <div
                                    key={module.id}
                                    onClick={() => handleToggleModule(module.id)}
                                    className={`cursor-pointer p-4 rounded-xl border transition-all flex items-center justify-between ${module.completed
                                        ? 'bg-slate-900/40 border-emerald-500/20 hover:border-emerald-500/40'
                                        : 'bg-slate-900/80 border-slate-800 hover:border-slate-700'
                                        }`}
                                >
                                    <div className="flex items-center gap-3.5">
                                        <button
                                            type="button"
                                            aria-label="Toggle completed"
                                            className={`p-1 rounded-full transition-colors ${module.completed ? 'text-emerald-400' : 'text-slate-600 hover:text-slate-400'
                                                }`}
                                        >
                                            {module.completed ? (
                                                <CheckCircle2 className="w-5 h-5 fill-emerald-500/20" />
                                            ) : (
                                                <Circle className="w-5 h-5" />
                                            )}
                                        </button>
                                        <div>
                                            <h4 className={`text-sm font-semibold ${module.completed ? 'line-through text-slate-400' : 'text-slate-200'}`}>
                                                {module.title}
                                            </h4>
                                            <span className="text-[10px] text-slate-400 uppercase tracking-wider">
                                                {module.badge || 'Core Module'}
                                            </span>
                                        </div>
                                    </div>

                                    <span className={`text-xs font-medium px-2.5 py-0.5 rounded-full ${module.completed ? 'bg-emerald-500/10 text-emerald-400' : 'bg-slate-800 text-slate-400'
                                        }`}>
                                        {module.completed ? 'Mastered' : 'In Progress'}
                                    </span>
                                </div>
                            ))}
                        </div>
                    </div>

                    {/* Performance & Score Trends Card */}
                    <div className="glass-panel rounded-2xl p-6 border border-slate-800/80">
                        <div className="flex items-center justify-between mb-4">
                            <div>
                                <h2 className="text-lg font-bold text-white flex items-center gap-2">
                                    <TrendingUp className="w-5 h-5 text-emerald-400" />
                                    <span>Interview Readiness Trend</span>
                                </h2>
                                <p className="text-xs text-slate-400 mt-0.5">
                                    NLP domain accuracy trajectory across consecutive mock interviews
                                </p>
                            </div>
                            <div className="flex items-center gap-2 text-xs font-semibold text-emerald-400 bg-emerald-500/10 px-3 py-1 rounded-full border border-emerald-500/30">
                                <span>+22% Growth</span>
                            </div>
                        </div>

                        {/* Custom Bar Graph Visualization */}
                        <div className="h-44 flex items-end justify-between gap-3 pt-6 pb-2 px-2 border-b border-slate-800">
                            {[
                                { label: 'May', value: 65 },
                                { label: 'Jun', value: 72 },
                                { label: 'Jul', value: 78 },
                                { label: 'Aug', value: 81 },
                                { label: 'Sep (Current)', value: 87 },
                            ].map((item, index) => (
                                <div key={index} className="flex-1 flex flex-col items-center gap-2 h-full justify-end group">
                                    <span className="text-[11px] font-bold text-brand-300 opacity-0 group-hover:opacity-100 transition-opacity">
                                        {item.value}%
                                    </span>
                                    <div
                                        className="w-full max-w-[48px] rounded-t-lg bg-gradient-to-t from-brand-600 to-indigo-500 group-hover:from-brand-500 group-hover:to-indigo-400 transition-all duration-300 shadow-md shadow-brand-500/20"
                                        style={{ height: `${item.value}%` }}
                                    />
                                    <span className="text-[11px] text-slate-400 font-medium truncate">
                                        {item.label}
                                    </span>
                                </div>
                            ))}
                        </div>
                        <div className="flex items-center justify-between mt-3 text-xs text-slate-400">
                            <span>Threshold for Amazon & TCS technical clears: 75%</span>
                            <span className="text-brand-400 font-medium">Target: 90%+</span>
                        </div>
                    </div>
                </div>

                {/* Right Column: Job Tracker, Quick Resume, Peer Feedback */}
                <div className="space-y-8">
                    {/* Job Application Tracker Widget */}
                    <div className="glass-panel rounded-2xl p-6 border border-slate-800/80">
                        <div className="flex items-center justify-between mb-4">
                            <h2 className="text-base font-bold text-white flex items-center gap-2">
                                <Briefcase className="w-4 h-4 text-brand-400" />
                                <span>Job Tracker Pipeline</span>
                            </h2>
                            <Link to="/jobs" className="text-xs font-semibold text-brand-400 hover:text-brand-300 flex items-center gap-1">
                                <span>View all</span>
                                <ArrowRight className="w-3 h-3" />
                            </Link>
                        </div>

                        <div className="space-y-3">
                            {jobTracker.slice(0, 3).map((job) => (
                                <div key={job.id} className="p-3.5 rounded-xl bg-slate-900/60 border border-slate-800 flex items-center justify-between">
                                    <div className="flex items-center gap-3">
                                        <div className="w-9 h-9 rounded-lg bg-slate-800 flex items-center justify-center text-slate-300 font-bold text-xs border border-slate-700">
                                            {job.company.substring(0, 2).toUpperCase()}
                                        </div>
                                        <div>
                                            <h4 className="text-xs font-bold text-white">{job.company}</h4>
                                            <p className="text-[11px] text-slate-400">{job.position}</p>
                                        </div>
                                    </div>

                                    <span className={`text-[10px] font-bold px-2 py-0.5 rounded-full border ${job.status === 'Interviewing'
                                        ? 'bg-amber-500/10 text-amber-300 border-amber-500/30'
                                        : job.status === 'Offer'
                                            ? 'bg-emerald-500/10 text-emerald-300 border-emerald-500/30'
                                            : 'bg-brand-500/10 text-brand-300 border-brand-500/30'
                                        }`}>
                                        {job.status}
                                    </span>
                                </div>
                            ))}
                        </div>
                    </div>

                    {/* Quick Resume Card */}
                    <div className="p-5 rounded-2xl bg-gradient-to-br from-slate-900 to-indigo-950/70 border border-slate-800">
                        <div className="flex items-start justify-between">
                            <div>
                                <span className="text-[10px] uppercase font-bold tracking-wider px-2 py-0.5 rounded bg-emerald-500/20 text-emerald-300 border border-emerald-500/30">
                                    ATS Score 92%
                                </span>
                                <h3 className="text-sm font-bold text-white mt-2">ATS-Optimized Resume</h3>
                                <p className="text-xs text-slate-400 mt-1">
                                    Ready for TCS Digital, Amazon campus, and SOC Analyst postings.
                                </p>
                            </div>
                            <div className="p-2.5 rounded-xl bg-indigo-500/10 text-indigo-400">
                                <FileText className="w-5 h-5" />
                            </div>
                        </div>
                        <Link
                            to="/resume"
                            className="mt-4 w-full py-2 px-3 rounded-lg text-xs font-semibold bg-slate-800 hover:bg-slate-700 text-slate-200 flex items-center justify-center gap-1.5 transition-colors"
                        >
                            <span>Edit & Export ATS Resume</span>
                            <ExternalLink className="w-3.5 h-3.5" />
                        </Link>
                    </div>

                    {/* Peer Feedback Highlights */}
                    <div className="glass-panel rounded-2xl p-6 border border-slate-800/80">
                        <div className="flex items-center justify-between mb-4">
                            <h2 className="text-base font-bold text-white flex items-center gap-2">
                                <Users className="w-4 h-4 text-purple-400" />
                                <span>Peer Feedback</span>
                            </h2>
                            <Link to="/feedback" className="text-xs font-semibold text-purple-400 hover:text-purple-300">
                                Hub
                            </Link>
                        </div>

                        <div className="space-y-3">
                            {peerFeedback.map((fb, idx) => (
                                <div key={idx} className="p-3 rounded-xl bg-slate-900/60 border border-slate-800 space-y-1.5">
                                    <div className="flex items-center justify-between">
                                        <span className="text-xs font-bold text-slate-300">{fb.reviewer}</span>
                                        <div className="flex items-center gap-1 text-amber-400 text-xs font-bold">
                                            <Star className="w-3.5 h-3.5 fill-amber-400 text-amber-400" />
                                            <span>{fb.rating}</span>
                                        </div>
                                    </div>
                                    <p className="text-xs text-slate-400 leading-snug">
                                        "{fb.comment}"
                                    </p>
                                </div>
                            ))}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    );
}
