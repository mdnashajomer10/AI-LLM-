# AI-LLM-
 GitHub project idea that combines LLMs, Diffusion Models, and real-world tools integration — designed to be visually compelling, technically impressive, and highly useful for the AI/dev community.
import React, { useState } from 'react';
import { Card, CardContent } from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Button } from '@/components/ui/button';
import { Textarea } from '@/components/ui/textarea';
import { Sparkles, ImageIcon } from 'lucide-react';
import { motion } from 'framer-motion';

export default function GenAgentStudio() {
  const [prompt, setPrompt] = useState('');
  const [response, setResponse] = useState('');
  const [imageUrl, setImageUrl] = useState('');
  const [loading, setLoading] = useState(false);

  const handlePromptSubmit = async () => {
    setLoading(true);
    setResponse('');
    setImageUrl('');

    const llmRes = await fetch('/api/generate-text', {
      method: 'POST',
      body: JSON.stringify({ prompt }),
    }).then(res => res.json());

    setResponse(llmRes.text);

    const imageRes = await fetch('/api/generate-image', {
      method: 'POST',
      body: JSON.stringify({ prompt }),
    }).then(res => res.json());

    setImageUrl(imageRes.url);
    setLoading(false);
  };

  return (
    <motion.div initial={{ opacity: 0 }} animate={{ opacity: 1 }} className="max-w-3xl mx-auto p-6">
      <h1 className="text-4xl font-bold mb-4 text-center">🧠 GenAgent Studio</h1>
      <Card className="mb-4">
        <CardContent className="space-y-4">
          <Textarea
            placeholder="Enter your creative or analytical prompt here..."
            value={prompt}
            onChange={e => setPrompt(e.target.value)}
            className="min-h-[120px]"
          />
          <Button onClick={handlePromptSubmit} disabled={loading}>
            {loading ? 'Thinking...' : 'Generate'}
          </Button>
        </CardContent>
      </Card>

      {response && (
        <motion.div initial={{ y: 20, opacity: 0 }} animate={{ y: 0, opacity: 1 }}>
          <Card className="mb-4">
            <CardContent>
              <h2 className="text-xl font-semibold flex items-center gap-2"><Sparkles className="w-5 h-5" />AI Response</h2>
              <p className="mt-2 whitespace-pre-wrap">{response}</p>
            </CardContent>
          </Card>
        </motion.div>
      )}

      {imageUrl && (
        <motion.div initial={{ y: 20, opacity: 0 }} animate={{ y: 0, opacity: 1 }}>
          <Card>
            <CardContent>
              <h2 className="text-xl font-semibold flex items-center gap-2"><ImageIcon className="w-5 h-5" />Generated Image</h2>
              <img src={imageUrl} alt="Generated" className="w-full mt-4 rounded-xl shadow" />
            </CardContent>
          </Card>
        </motion.div>
      )}
    </motion.div>
  );
}
